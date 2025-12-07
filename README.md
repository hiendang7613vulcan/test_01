Tôi sẽ phân tích hệ thống này để tạo bản thảo chi tiết. Để làm điều này, tôi cần đọc các file quan trọng trong project.

[12 tools called]

Dựa trên việc phân tích codebase, tôi sẽ tạo **Bản Thảo Kiến Trúc Hệ Thống GrandEco**:

---

# 📋 GrandEco System Architecture - Bản Thảo Chi Tiết

## 🎯 Tổng Quan Hệ Thống

**GrandEco** là một **Marketing AI Operating System** cho Apple Search Ads, gồm các module chính:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          GrandEco Platform                              │
├─────────────────┬─────────────────┬─────────────────┬───────────────────┤
│   Kursor Ads    │   GrandStream   │     ActPort     │    ChatSmile      │
│  (ASA Command)  │ (Signals+Stories)│ (Task Execution)│  (AI Assistant)   │
└─────────────────┴─────────────────┴─────────────────┴───────────────────┘
```

---

## 📥 INPUT - Nguồn Dữ Liệu

### 1. **Apple Search Ads (ASA) API**
- **Endpoint Base**: `https://api.searchads.apple.com/api/v5`
- **Authentication**: OAuth2 / JWT (ES256)
- **Backend File**: `backend/asa_data.py`

**Dữ liệu lấy được:**
| Entity | Fields |
|--------|--------|
| **Campaigns** | id, name, status, dailyBudget, countriesOrRegions |
| **Ad Groups** | id, campaignId, name, status, defaultBidAmount |
| **Keywords** | id, adGroupId, text, matchType, status, bidAmount |
| **Search Terms** | searchTermText, matchType, impressions, taps, installs, spend |
| **Reports** | impressions, taps, installs, spend, avgCPI, ttr, conversionRate, returnOnAdSpend |

### 2. **AppsFlyer API** (Single Source of Truth)
- **Backend File**: `backend/appsflyer_data.py`

**Dữ liệu lấy được:**
| Type | Fields |
|------|--------|
| **Installs** | install_time, media_source, campaign, country, device_type, revenue, cost |
| **Events** | event_time, event_name, media_source, campaign, revenue |
| **Attribution** | media_source, campaign, installs, impressions, clicks, cost, revenue, cpi, roas |
| **Cohorts** | cohort_day, retained_users, retention_rate, revenue, ltv |
| **Cost Data** | date, media_source, campaign, impressions, clicks, cost |

### 3. **RSS Feeds** (Market Stories)
- **Default Sources**:
  - TechCrunch Mobile
  - Mobile Dev Memo  
  - 9to5Mac
  - MacRumors
  - Search Engine Land
  - AdExchanger
  - Sensor Tower Blog

---

## ⚙️ PROCESS - Xử Lý Dữ Liệu

### Module Architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                        Backend (FastAPI)                            │
│  ┌──────────────┐  ┌───────────────┐  ┌────────────────────────┐  │
│  │  ASA Router   │  │ AppsFlyer     │  │   Signal Engine        │  │
│  │  (asa_data)   │  │ (appsflyer)   │  │   (signal_engine.py)   │  │
│  └──────┬───────┘  └───────┬───────┘  └───────────┬────────────┘  │
│         │                  │                      │               │
│         ▼                  ▼                      ▼               │
│  ┌───────────────────────────────────────────────────────────────┐│
│  │                    Database (SQLite)                          ││
│  │  metrics_daily | signals | tasks | stories | automation_rules ││
│  └───────────────────────────────────────────────────────────────┘│
│         │                  │                      │               │
│         ▼                  ▼                      ▼               │
│  ┌──────────────┐  ┌───────────────┐  ┌────────────────────────┐ │
│  │   Tasks API   │  │  Stories API  │  │      Chat API          │ │
│  │  (tasks.py)   │  │ (stories.py)  │  │     (chat.py)          │ │
│  └──────────────┘  └───────────────┘  └────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

### Signal Detection Engine (`signal_engine.py`)

**Loại Signal phát hiện:**

| Signal Type | Threshold | Severity | Description |
|-------------|-----------|----------|-------------|
| `cpi_spike` | +20% | warning/critical | CPI tăng đột biến |
| `cpi_drop` | -20% | info | CPI giảm (tốt) |
| `roas_anomaly` | -20% | warning | ROAS giảm |
| `install_drop` | -25% | critical | Lượng install giảm |
| `budget_pacing` | >100% | warning | Chi tiêu vượt budget |
| `keyword_underperform` | >2x avg | warning | Keyword CPI cao |
| `creative_fatigue` | -25% CTR | warning | CTR creative giảm |
| `creative_winner` | >1.5x avg | info | Creative hiệu quả |

**Phương thức so sánh:**
- **Day-over-Day**: So với ngày hôm trước
- **7-Day Average**: So với trung bình 7 ngày

---

## 🗄️ DATABASE - SQLite Schema

**File**: `backend/grandstream.db`

### Tables Structure

```sql
-- Metrics hàng ngày
metrics_daily (
    id, date, entity_type, entity_id, entity_name,
    impressions, taps, installs, spend, cpi, ctr,
    conversion_rate, roas, daily_budget
)

-- Signals phát hiện được
signals (
    id, type, severity, entity_type, entity_id, entity_name,
    metric, current_value, baseline_value, change_pct,
    comparison_type, detected_at, status, suggested_action, task_id
)

-- Tasks queue (ActPort)
tasks (
    id, name, description, type,
    app_id, campaign_id, ad_group_id, keyword_id, country,
    source, source_detail, changes (JSON),
    status, priority, external,
    approved_by, approved_at, rejected_reason,
    executed_at, error
)

-- Automation rules
automation_rules (
    id, name, description, enabled,
    scope_type, scope_ids (JSON),
    conditions (JSON), condition_logic,
    signal_type, signal_severity,
    last_triggered_at, trigger_count
)

-- AI-generated stories
stories (
    id, title, summary, full_content,
    importance, priority, category, tags (JSON),
    impacts (JSON), source_urls (JSON), source_titles (JSON),
    source_feeds (JSON)
)

-- RSS sources
rss_sources (id, name, url, category, enabled, last_crawled_at)

-- Chat history
chat_history (id, session_id, role, content, module_context)
```

---

## 🔌 BACKEND API ENDPOINTS

**Base URL**: `http://localhost:8000`

### Core APIs

| Module | Prefix | Key Endpoints |
|--------|--------|---------------|
| **Health** | `/` | `GET /` - Status check |
| **MindFlow** | - | `POST /think`, `POST /regenerate`, `POST /answer` |
| **Chat** | `/chat` | `POST /chat` (SSE streaming) |
| **Tasks** | `/tasks` | CRUD + `/approve`, `/reject`, `/retry`, `/batch/*` |
| **Rules** | `/rules` | CRUD + `/toggle`, `/evaluate`, `/test` |
| **Stories** | `/stories` | CRUD + `/crawl`, `/sources` |
| **Signals** | `/signals` | CRUD + `/acknowledge`, `/dismiss`, `/create-task`, `/detect` |
| **ASA** | `/asa` | `/status`, `/campaigns`, `/adgroups`, `/keywords`, `/sync`, `/reports/*` |
| **AppsFlyer** | `/appsflyer` | `/status`, `/installs`, `/events`, `/attribution`, `/cohorts`, `/sync` |
| **Forecast** | `/forecast` | `/status`, `/metrics/{metric}`, `/summary`, `/batch` |

### Write Operations (ASA)

```
PUT /asa/campaigns/{id}/budget          - Update budget
PUT /asa/campaigns/{id}/status          - Pause/Enable campaign
PUT /asa/campaigns/{id}/adgroups/{id}/keywords/{id}/bid    - Update bid
PUT /asa/campaigns/{id}/adgroups/{id}/keywords/{id}/status - Pause/Enable keyword
```

---

## 🖥️ FRONTEND PAGES

### Application Routes (`app/(app)/`)

| Route | Page | Description |
|-------|------|-------------|
| `/kursorads` | **Kursor Ads** | ASA Command Center - Performance tables, Search Terms, Cohorts, Signals |
| `/grandstream` | **GrandStream** | Signal & Story Radar - Timeline, Stories list, Signals table |
| `/actport` | **ActPort** | Task Queue - Pending approvals, Running tasks, History |
| `/chatsmile` | **ChatSmile** | AI Assistant - Context-aware chat panel |

### Key Components

```
components/
├── kursor-ads/          # Performance tables, Campaign tree, Cohort table
├── grandstream/         # Signal cards, Signal list
├── actport/             # Task cards
├── chat/                # Chat input, Chat message
├── layout/              # App shell, ChatSmile panel
└── ui/                  # Buttons, Cards, Dropdowns, etc.
```

---

## 🔄 DATA FLOW - Luồng Dữ Liệu

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                              DATA FLOW                                        │
│                                                                              │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐   │
│  │ AppsFlyer   │    │ ASA API     │    │ RSS Feeds   │    │ User Input  │   │
│  │ Pull API    │    │ v5          │    │             │    │ (Chat/UI)   │   │
│  └──────┬──────┘    └──────┬──────┘    └──────┬──────┘    └──────┬──────┘   │
│         │                  │                  │                  │          │
│         ▼                  ▼                  ▼                  ▼          │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                    Backend Routers (FastAPI)                         │   │
│  │  appsflyer_data.py | asa_data.py | stories.py | chat.py | tasks.py   │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│         │                  │                  │                  │          │
│         ▼                  ▼                  ▼                  ▼          │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                          SQLite Database                              │   │
│  │   metrics_daily | signals | tasks | stories | automation_rules        │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                              │                                              │
│         ┌────────────────────┼────────────────────┐                        │
│         ▼                    ▼                    ▼                        │
│  ┌─────────────┐    ┌─────────────────┐   ┌─────────────────┐             │
│  │Signal Engine│    │ OpenAI GPT-4.1  │   │  Prophet Model  │             │
│  │(Anomalies)  │    │ (Chat/Stories)  │   │  (Forecasting)  │             │
│  └─────────────┘    └─────────────────┘   └─────────────────┘             │
│         │                    │                    │                        │
│         ▼                    ▼                    ▼                        │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                      Frontend (Next.js)                               │   │
│  │  Kursor Ads | GrandStream | ActPort | ChatSmile                       │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 OUTPUT - Kết Quả

### 1. **Kursor Ads Page** (`/kursorads`)
- Performance table với groupby: Campaign/AdGroup/Keyword/Country
- Search Terms với suggested actions (Add Exact/Broad/Negative)
- Cohort & LTV analysis
- Inline editing → Create Task

### 2. **GrandStream Page** (`/grandstream`)  
- Timeline: Stories + Signals unified view
- Stories grid/list với importance scoring
- Signals table với severity, status filters
- Actions: Acknowledge, Dismiss, Create Task

### 3. **ActPort Page** (`/actport`)
- Task queue với status: Pending, Approved, Running, Completed, Failed
- Batch approve/reject
- Task detail: Changes diff, Execution logs
- Source tracking: kursor_ads, grandstream, chatsmile, manual

### 4. **ChatSmile Panel**
- Context-aware AI assistant
- Module contexts: kursor_ads, grandstream, actport
- Task extraction từ conversation
- Streaming responses

---

## 🔑 Environment Variables Required

```env
# OpenAI
OPENAI_API_KEY=sk-...
LLM_MODEL=gpt-4.1-2025-04-14

# Apple Search Ads (OAuth or JWT)
ASA_CLIENT_ID=
ASA_CLIENT_SECRET=
ASA_ORG_ID=
ASA_TEAM_ID=
ASA_KEY_ID=
ASA_PRIVATE_KEY=

# AppsFlyer
AF_APP_ID=
AF_API_TOKEN=
AF_PULL_API_TOKEN=
```

---

Đây là bản thảo tổng quan về hệ thống GrandEco. Bạn cần tôi đi sâu hơn vào phần nào không?
