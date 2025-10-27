
# Báo cáo Tổng quan Repository: AI Note Taker Service

## 1. Giới thiệu và Mục tiêu Dự án

AI Note Taker Service là một microservice Python do Vulcanlabs AI Team phát triển, tập trung vào chuyển đổi audio thành văn bản (ASR), tóm tắt nội dung, tạo quiz/flashcard từ audio, PDF và webpage. Dự án đồng thời đóng vai trò template mẫu cho các microservices nội bộ, hướng tới triển khai nhanh, mở rộng và kiểm thử trong môi trường phát triển AI hiện đại.

### Mục tiêu chính:
- Tự động hóa chuyển đổi audio thành text, tóm tắt, sinh quiz/flashcard.
- Hỗ trợ đa nền tảng (Android/iOS/Web).
- Tích hợp kiểm duyệt nội dung NSFW đa ngôn ngữ.
- Xử lý bất đồng bộ với Google Cloud Pub/Sub.
- Đáp ứng tiêu chuẩn production: REST API, Prometheus metrics, Docker hóa, logging, monitoring.

## 2. Kiến trúc & Công nghệ

### Tech Stack:
- **Ngôn ngữ:** Python (~100%)
- **Framework:** FastAPI, Uvicorn
- **AI/ML:** OpenAI GPT-4, Mistral AI (OCR), Whisper v3 (ASR), Fireworks, Groq
- **Database:** Redis (cache, whitelist, process status)
- **Message Queue:** Google Cloud Pub/Sub
- **Container:** Docker
- **Monitoring:** Prometheus, Loguru

### Kiến trúc tổng thể:
- REST API với FastAPI, chia nhỏ theo routers cho từng chức năng (ASR, summary, quiz, flashcard, PDF/webpage summary, NSFW whitelist...)
- Xử lý bất đồng bộ qua Pub/Sub subscriber (audio processing pipeline)
- Tách biệt rõ business logic (services), orchestration (usecases), model wrappers, và database layer
- Demo UI với Streamlit phục vụ kiểm thử nhanh

## 3. Cấu trúc Thư mục & Tệp Trọng Yếu

- **main.py:** Entrypoint chính, khởi động server và Pub/Sub subscriber.
- **source/app.py:** Định nghĩa FastAPI app, đăng ký routers, middleware, load NSFW whitelist.
- **source/configs/config.yaml:** Cấu hình model (Whisper, GPT-4, Mistral), tham số AI.
- **source/endpoint/schemas.py:** Pydantic schemas cho request/response, đảm bảo type safety.
- **source/pubsub_app.py:** Xử lý message từ Pub/Sub, trigger pipeline ASR + summary.
- **source/usecases/process_handler.py:** Orchestration cho các background task.
- **source/dbs/redis/nsfw_whitelist_manager.py:** Quản lý whitelist kiểm duyệt nội dung.
- **requirements.txt:** Định nghĩa dependencies.
- **deploy/docker/Dockerfile:** Docker hóa service.

## 4. Tính năng Chính & API

### Tính năng:
- **ASR + Tóm tắt audio:** Chuyển audio thành text, tóm tắt nội dung.
- **Tóm tắt PDF/webpage:** OCR + LLM tóm tắt tài liệu số.
- **Sinh quiz/flashcard:** Từ transcript audio hoặc text.
- **Kiểm duyệt NSFW:** Whitelist đa ngôn ngữ, quản lý qua API/CLI.
- **Xử lý bất đồng bộ:** Qua Pub/Sub, hỗ trợ scale.
- **Monitoring:** Prometheus metrics, health check endpoint.

### API Endpoints (tiêu biểu):
| Method | Path | Mục đích |
|--------|------|----------|
| POST | /api/v1/internal/audio-to-text | ASR + tóm tắt audio |
| POST | /api/v1/internal/summary-generation | Tóm tắt transcript |
| POST | /api/v1/internal/quizzes-generation | Sinh quiz |
| POST | /api/v1/internal/flash-cards-generation | Sinh flashcard |
| POST | /api/v1/internal/pdf-summary-generation | Tóm tắt PDF |
| POST | /api/v1/internal/webpage-summary-generation | Tóm tắt webpage |
| GET  | /api/v1/internal/process/{id}/status | Kiểm tra trạng thái task |
| GET  | /api/status | Health check |
| GET  | /api/metrics | Prometheus metrics |

### CLI Tools:
- Quản lý NSFW whitelist (load, add, stats...)
- Generate Swagger docs

## 5. AI Models & Data Flow

- **ASR:** Whisper v3 (Fireworks, Groq)
- **Summarization:** GPT-4 (OpenAI)
- **Quiz/Flashcard:** GPT-4o
- **OCR:** Mistral
- **VAD:** pyannote/segmentation-3.0
- **Translation:** GPT-4.1-mini

#### Model selection linh hoạt theo platform (Android/iOS), fallback thông minh.

#### Redis lưu trạng thái process, NSFW whitelist dạng set theo ngôn ngữ.

## 6. Vận hành & Quickstart

- Yêu cầu: Python 3.11+, ffmpeg, Redis, Google Cloud Pub/Sub credentials (nếu dùng async)
- Cấu hình qua .env (tokens, Redis, backend URLs...)
- Chạy local: `python3 main.py` (API tại http://0.0.0.0:8080)
- Docker hóa: build/run image với Dockerfile
- Demo UI: Streamlit (cảnh báo chi phí API thật)

## 7. Bảo mật, Giấy phép & Rủi ro

- **License:** Proprietary (Vulcanlabs)
- **Security Risks:**
  - Hardcoded secrets (cần kiểm soát .env)
  - SSRF risk với các endpoint nhận public URL (audio, PDF, webpage)
  - Redis không password = exposed
  - Log level DEBUG có thể leak secrets
- **Dependency Risks:**
  - Không có lock file → khó kiểm soát reproducibility
  - Một số package version cũ có thể có CVE

## 8. Hạn chế, Nợ kỹ thuật & Đề xuất cải tiến

- **Thiếu test coverage:** Chưa có unit/integration tests
- **Chưa có CI/CD:** Không phát hiện workflow tự động hóa build/lint/test
- **Chưa có rate limiting:** API dễ bị abuse
- **Error handling:** Chưa rõ retry logic với external API
- **Magic numbers, typo, duplicate imports:** Cần refactor
- **Swagger UI:** Chưa enable /docs, chỉ có static JSON
- **Monitoring:** Có Prometheus, loguru nhưng health check chưa kiểm tra Redis/PubSub

### Đề xuất cải tiến:
- Thêm test suite (pytest), CI/CD (GitHub Actions)
- Enable Swagger UI, bổ sung OpenAPI docs
- Implement rate limiting, validate URL input
- Tách secrets khỏi code, thêm .env.example
- Fix typo, magic numbers, duplicate code
- Bổ sung health check cho các service phụ trợ

## 9. Kết luận & Giá trị học thuật

AI Note Taker Service là ví dụ điển hình về microservice AI production-grade, tích hợp đa mô hình AI, xử lý bất đồng bộ, kiểm duyệt nội dung, monitoring và containerization. Dự án phù hợp cho nghiên cứu, học tập về kiến trúc microservice, orchestration AI pipeline, bảo mật API, và CI/CD hiện đại. Tuy nhiên, để đạt chuẩn production thực sự, cần bổ sung test coverage, CI/CD, rate limiting và kiểm soát bảo mật nghiêm ngặt hơn.

### Từ khóa: ASR, OCR, LLM, microservice, FastAPI, Prometheus, Pub/Sub, NSFW, containerization, AI pipeline.

