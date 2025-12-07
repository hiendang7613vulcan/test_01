# ASR API Comparison: Quality, Price & Latency

**Research Date:** December 2025  
**Sources:** Vendor documentation, independent benchmarks, third-party comparisons

---

## Master Comparison Table

| Provider | Model | Price/min | Price/hr | Streaming | Latency | WER (English) | Diarization | Languages | Best For |
|----------|-------|-----------|----------|-----------|---------|---------------|-------------|-----------|----------|
| **AssemblyAI** | Universal-2 | $0.0025 | $0.15 | ✅ | <300ms | ~6.6% | ✅ 50 spk | 20+ | Best value + AI features |
| **Deepgram** | Nova-3 | $0.0043 | $0.26 | ✅ | <300ms | ~7-8% | ✅ 10+ spk | 36 | Speed + production scale |
| **OpenAI** | Whisper | $0.006 | $0.36 | ❌ | Batch | ~7.9% | ❌ | 99+ | Open-source compatible |
| **OpenAI** | GPT-4o-transcribe | $0.006 | $0.36 | ❌ | Batch | ~5-6% | ✅ | Multi | Improved accuracy |
| **OpenAI** | GPT-4o-mini-transcribe | $0.003 | $0.18 | ❌ | Batch | ~8-9% | ❌ | Multi | Budget option |
| **ElevenLabs** | Scribe v1 | $0.0067 | $0.40 | ❌ | Batch | ~3.3% | ✅ 32 spk | 99 | Highest accuracy |
| **ElevenLabs** | Scribe v2 Realtime | ~$0.0067 | ~$0.40 | ✅ | <150ms | ~3-4% | ✅ | 90+ | Ultra-low latency |
| **Google** | Chirp 3 | $0.016 | $0.96 | ✅ | Varies | ~9-10% | ✅ 10+ | 70+ | GCP ecosystem |
| **Google** | Standard | $0.006 | $0.36 | ✅ | Varies | ~12-15% | ✅ | 125+ | Budget GCP |
| **Gladia** | Solaria | $0.0102 | $0.61 | ✅ | ~270ms | ~6% (94% acc) | ✅ | 100+ | Multilingual |
| **Gladia** | Whisper-Zero | $0.0102 | $0.61 | ✅ | <300ms | ~7-8% | ✅ | 100+ | Zero hallucination |
| **Azure** | Speech Services (Batch) | $0.006 | $0.36 | ❌ | Batch | ~10-12% | ✅ | 110+ | Microsoft ecosystem |
| **Azure** | Speech Services (RT) | $0.017 | $1.02 | ✅ | Low | ~10-12% | ✅ | 110+ | Teams integration |
| **AWS** | Transcribe | $0.024 | $1.44 | ✅ | Medium | ~10-12% | ✅ 10 spk | 100+ | AWS ecosystem |
| **AWS** | Transcribe Medical | $0.075 | $4.50 | ✅ | Medium | ~5-8% | ✅ | Limited | Healthcare |
| **Speechmatics** | Enhanced | $0.004 | $0.24 | ✅ | Low | ~8-10% | ✅ | 50+ | On-prem option |
| **Groq** | Whisper v3 Turbo | $0.00067 | $0.04 | ❌ | Fast | ~8% | ❌ | 99+ | Cheapest hosted |
| **fal.ai** | Wizper V3 | $0.0005 | $0.03 | ❌ | Fast | ~8-9% | ❌ | 99+ | Ultra budget |

---

## Detailed Analysis by Category

### 💰 Price Rankings (Low to High)

| Rank | Provider | Model | $/minute | $/hour | Notes |
|------|----------|-------|----------|--------|-------|
| 1 | fal.ai | Wizper V3 | $0.0005 | $0.03 | Cheapest, no diarization |
| 2 | Groq | Whisper v3 Turbo | $0.00067 | $0.04 | Very fast inference |
| 3 | **AssemblyAI** | Universal-2 | **$0.0025** | **$0.15** | **Best value with features** |
| 4 | OpenAI | GPT-4o-mini | $0.003 | $0.18 | Budget option |
| 5 | Speechmatics | Enhanced | $0.004 | $0.24 | On-prem available |
| 6 | **Deepgram** | Nova-3 | **$0.0043** | **$0.26** | **Production favorite** |
| 7 | OpenAI | Whisper/GPT-4o | $0.006 | $0.36 | Standard pricing |
| 8 | Azure | Batch | $0.006 | $0.36 | Microsoft ecosystem |
| 9 | Google | Standard | $0.006 | $0.36 | GCP required |
| 10 | ElevenLabs | Scribe | $0.0067 | $0.40 | Highest accuracy |
| 11 | Gladia | Solaria | $0.0102 | $0.61 | 100+ languages |
| 12 | Google | Chirp 3 | $0.016 | $0.96 | Premium GCP |
| 13 | Azure | Real-time | $0.017 | $1.02 | Streaming premium |
| 14 | AWS | Transcribe | $0.024 | $1.44 | AWS ecosystem tax |

---

### ⚡ Latency Rankings (Streaming)

| Rank | Provider | Latency | Notes |
|------|----------|---------|-------|
| 1 | **ElevenLabs Scribe v2** | **<150ms** | Fastest streaming, "negative latency" |
| 2 | Gladia Solaria | ~270ms | Consistent P50 |
| 3 | Deepgram Nova-3 | <300ms | P50, handles scale |
| 4 | AssemblyAI Universal-2 | <300ms | P50, consistent |
| 5 | Gladia Whisper-Zero | <300ms | Stable |
| 6 | Speechmatics | Low | Not specified |
| 7 | Azure Real-time | Low-Medium | Variable |
| 8 | Google Chirp 3 | Varies | Can be inconsistent |
| 9 | AWS Transcribe | Medium | ~500ms+ typical |

**Batch-only (no streaming):**
- OpenAI Whisper/GPT-4o: 5-10x realtime processing
- Azure Batch: Variable
- fal.ai/Groq: Fast batch processing

---

### 🎯 Accuracy Rankings (WER - Lower is Better)

| Rank | Provider | Model | WER (English) | Notes |
|------|----------|-------|---------------|-------|
| 1 | **ElevenLabs** | Scribe v1/v2 | **~3.3%** | Best accuracy (96.7% acc) |
| 2 | OpenAI | GPT-4o-transcribe | ~5-6% | Improved over Whisper |
| 3 | AWS | Transcribe Medical | ~5-8% | Domain-specific |
| 4 | Gladia | Solaria | ~6% | 94% accuracy claimed |
| 5 | **AssemblyAI** | Universal-2 | **~6.6%** | Consistent across datasets |
| 6 | **Deepgram** | Nova-3 | **~7-8%** | Good noise handling |
| 7 | OpenAI | Whisper Large-v3 | ~7.9% | Open-source baseline |
| 8 | Groq/fal.ai | Whisper hosted | ~8-9% | Same as Whisper |
| 9 | Speechmatics | Enhanced | ~8-10% | Good for accents |
| 10 | Google | Chirp 3 | ~9-10% | Variable |
| 11 | Azure | Speech Services | ~10-12% | Enterprise focus |
| 12 | AWS | Transcribe | ~10-12% | AWS ecosystem |
| 13 | Google | Standard | ~12-15% | Basic tier |

**Note:** WER varies significantly based on audio quality, accents, and domain. Always test with your own data.

---

### 🗣️ Diarization Comparison

| Provider | Max Speakers | Quality | Streaming Diar | Notes |
|----------|--------------|---------|----------------|-------|
| **AssemblyAI** | **50** | Excellent | ✅ | Best speaker counting (2.9% error) |
| ElevenLabs | 32 | Excellent | ❌ (v1), ✅ (v2) | Audio event tagging |
| Gladia | Multi | Good | ✅ | Pyannote-based |
| Deepgram | 10+ | Good | ✅ | Real-time labels |
| Google Chirp | 10+ | Good | ✅ | Auto punctuation |
| Azure | Multi | Good | ✅ | Teams optimized |
| AWS | 10 | Moderate | ✅ | Limited speakers |
| OpenAI GPT-4o | Yes | Good | ❌ | Batch only, new feature |
| OpenAI Whisper | ❌ | N/A | ❌ | No native diarization |

---

### 🌍 Language Support

| Provider | Languages | Vietnamese | Best Languages |
|----------|-----------|------------|----------------|
| Google | 125+ | ✅ Likely | All major |
| Azure | 110+ | Check | European, Asian |
| Gladia | 100+ | ✅ | 42 unique languages |
| AWS | 100+ | Check | Major languages |
| OpenAI Whisper | 99+ | ✅ | Major languages |
| ElevenLabs | 99 | ✅ (excellent) | 25+ excellent tier |
| Speechmatics | 50+ | Check | European focus |
| Deepgram | 36 | Check | English optimized |
| AssemblyAI | 20+ | Check | English best |

---

## Cost Calculator (Monthly Estimates)

### 100 Hours/Month Audio

| Provider | Base Cost | With Diarization | Total |
|----------|-----------|------------------|-------|
| fal.ai Wizper | $3 | N/A | $3 |
| Groq Whisper | $4 | N/A | $4 |
| **AssemblyAI** | **$15** | **Included** | **$15** |
| Deepgram Nova-3 | $26 | Included | $26 |
| OpenAI Whisper | $36 | +$30 (external) | $66 |
| ElevenLabs | $40 | Included | $40 |
| Gladia | $61 | Included | $61 |
| Google Chirp | $96 | Included | $96 |
| Azure RT | $102 | Included | $102 |
| AWS Transcribe | $144 | Included | $144 |

### 1,000 Hours/Month Audio

| Provider | Monthly Cost | Notes |
|----------|--------------|-------|
| fal.ai | $30 | No diarization |
| Groq | $40 | No diarization |
| **AssemblyAI** | **$150** | **All features included** |
| Deepgram | $260 | Volume discounts available |
| OpenAI | $360 | + diarization if needed |
| ElevenLabs | $400 | Premium accuracy |
| Gladia | $610 | 100+ languages |
| Google Chirp | $960 | GCP ecosystem |
| AWS | $1,440 | AWS ecosystem |

---

## Feature Matrix

| Feature | AssemblyAI | Deepgram | ElevenLabs | OpenAI | Google | Gladia | AWS | Azure |
|---------|------------|----------|------------|--------|--------|--------|-----|-------|
| **Streaming** | ✅ | ✅ | ✅ v2 | ❌ | ✅ | ✅ | ✅ | ✅ |
| **Diarization** | ✅ | ✅ | ✅ | ✅ GPT-4o | ✅ | ✅ | ✅ | ✅ |
| **Word Timestamps** | ✅ | ✅ | ✅ | ✅ Whisper | ✅ | ✅ | ✅ | ✅ |
| **Sentiment** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **PII Redaction** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **Topic Detection** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Summarization** | ✅ LeMUR | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Custom Vocab** | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| **On-Prem** | ❌ | ✅ | ❌ | OSS | ✅ | ✅ | ❌ | ✅ |
| **HIPAA** | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | ✅ |
| **SOC 2** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Recommendations by Use Case

### 🏆 Best Overall Value
**AssemblyAI Universal-2** - $0.15/hr
- Best price-to-feature ratio
- Excellent accuracy (~6.6% WER)
- 50 speaker diarization
- Built-in AI features (sentiment, topics, summary)
- <300ms streaming latency

### ⚡ Best for Speed/Scale
**Deepgram Nova-3** - $0.26/hr
- Sub-300ms consistent latency
- Handles thousands of concurrent streams
- Custom model training
- On-prem deployment option
- Real-time keyword prompting

### 🎯 Best Accuracy
**ElevenLabs Scribe** - $0.40/hr
- Lowest WER (~3.3%)
- 99 languages with excellent tier
- Audio event tagging
- <150ms latency (v2 Realtime)
- 32 speaker diarization

### 💰 Best Budget (with features)
**AssemblyAI Universal-2** - $0.15/hr
- Cheapest full-featured option
- All intelligence features included

### 💰 Best Ultra-Budget (basic)
**Groq Whisper** - $0.04/hr
- Cheapest hosted Whisper
- No diarization
- Fast batch processing

### 🌍 Best Multilingual
**Gladia Solaria** - $0.61/hr
- 100+ languages
- 42 unique languages not supported elsewhere
- 270ms latency
- Good for global enterprises

### 🏢 Best Enterprise/Compliance
**Azure Speech Services** or **AWS Transcribe**
- Deep ecosystem integration
- Enterprise compliance (HIPAA, SOC 2)
- On-prem options
- Higher cost but integrated billing

### 🇻🇳 Best for Vietnamese
1. **ElevenLabs Scribe** - Listed as "excellent" tier
2. **OpenAI Whisper** - Good Vietnamese support
3. **Google Chirp 3** - 70+ languages likely includes VN
4. **Gladia** - 100+ languages

---

## Quick Decision Guide

```
Need streaming + diarization?
├── Budget priority → AssemblyAI ($0.15/hr)
├── Latency priority → ElevenLabs v2 (<150ms)
├── Scale priority → Deepgram Nova-3
└── Enterprise → Azure/AWS

Need highest accuracy?
├── Cost not concern → ElevenLabs (3.3% WER)
├── Good balance → AssemblyAI (6.6% WER)
└── Budget → Groq Whisper (8% WER)

Need batch processing only?
├── Cheapest → fal.ai/Groq ($0.03-0.04/hr)
├── With diarization → OpenAI GPT-4o ($0.36/hr)
└── Best accuracy → ElevenLabs ($0.40/hr)

Already in cloud ecosystem?
├── AWS → AWS Transcribe
├── GCP → Google Chirp 3
├── Azure → Azure Speech Services
└── None → AssemblyAI or Deepgram
```

---

## Sources & Methodology

**Pricing Data:** Vendor documentation, verified December 2025

**WER Benchmarks:** 
- AssemblyAI official benchmarks (Oct 2025)
- Ionio.ai independent benchmark (Jul 2025)
- Voice Writer benchmark (Jan 2025)
- Vendor self-reported (noted where applicable)

**Latency Data:** Vendor documentation and community benchmarks

**Note:** Prices and specs change frequently. Always verify with vendors before production decisions.

---

**Last Updated:** December 8, 2025
