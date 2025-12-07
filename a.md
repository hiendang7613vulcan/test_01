# ASR + Speaker Diarization: Bảng Tổng Hợp Toàn Diện

**Cập nhật:** December 2025

---

## Bảng So Sánh Tất Cả Giải Pháp

| Solution | Type | Streaming | Diarization | Latency | Languages | Vietnamese | Cost | License/Pricing | Best For | Limitations |
|----------|------|-----------|-------------|---------|-----------|------------|------|-----------------|----------|-------------|
| **WhisperX** | OSS Batch | ❌ | ✅ (pyannote) | 70x RT | 99+ | ✅ Good | GPU only | BSD-4-Clause | Meeting/podcast post-processing | No streaming, dependency hell |
| **pyannote 4.0** | OSS Diarization | ❌ | ✅ SOTA | 31s/hr (H100) | Agnostic | ✅ | GPU only | MIT | Research, custom pipeline | Offline only |
| **Diart** | OSS Streaming | ✅ | ✅ | 500ms-5s | Agnostic | ✅ | GPU optional | MIT | Live diarization, group calls | Trade-off latency vs quality |
| **Reverb (Rev)** | OSS Pipeline | ❌ | ✅ v2 WavLM | Fast | EN only | ❌ | GPU only | Non-commercial | Long-form English, benchmark | English only, non-commercial |
| **SenseVoice** | OSS ASR | ✅ | ❌ | Very low | ZH/EN/VI | ✅ Very Good | GPU optional | Apache 2.0 | Emotion detection, fast ASR | No native diarization |
| **NeMo Canary-Qwen** | OSS ASR | ❌ | ❌ | 418x RT | EN only | ❌ | GPU required | Apache 2.0 | SOTA English accuracy | English only |
| **NeMo Sortformer v2** | OSS Streaming | ✅ | ✅ E2E | 20-40ms | EN/ZH | ❌ | GPU required | CC-BY-NC-4.0 | Real-time diar (2-4 spk) | Max 4 speakers, GPU only |
| **Voxtral** | OSS ASR+LLM | ✅ (API) | ❌ | Low | 8 langs | ❌ | GPU/Cloud | Apache 2.0 | ASR + understanding | No diarization, no Vietnamese |
| **IBM Granite 3.3** | OSS ASR | ✅ | ❌ | Medium | 8 langs | ❌ | GPU | Apache 2.0 | Enterprise, translation | No diarization |
| **Sherpa-onnx** | OSS Edge | ✅ | ✅ VAD-based | Low | Multiple | ✅ Good | CPU (mobile) | Apache 2.0 | Mobile/IoT offline | Complex setup |
| **Picovoice Falcon** | Commercial Edge | ✅ | ✅ On-device | Real-time | Agnostic | ✅ | Freemium | Commercial | Mobile privacy, any ASR | Accuracy vs cloud |
| **parakeet-rs** | OSS Rust | ✅ | ✅ Sortformer | ~160ms | 25 langs | Check | CPU/GPU | MIT/Apache | Rust backend, CPU-friendly | Max 4 speakers |
| **Qwen2-Audio** | OSS Audio-LLM | ❌ | ✅ via prompt | Medium | Multi | Check | GPU 16GB+ | Open | E2E understanding, no pipeline | Heavy GPU requirement |
| **Ultravox** | OSS Audio-LLM | ❌ | ❌ | Medium | Multi | Check | GPU | Open-weights | Semantic understanding | Lower word-level accuracy |
| **Moshi** | OSS S2S | ✅ | ✅ implicit | <200ms | Multi | Check | GPU | Open | Real-time S2S, overlap handling | Experimental |
| **Deepgram Nova-3** | API | ✅ | ✅ | <300ms | 36 | Check | $0.0043-0.0077/min | Commercial | Production, high volume | - |
| **AssemblyAI Universal-2** | API | ✅ | ✅ | <300ms | 16 | Check | $0.0025/min | Commercial | Best value, AI features | Fewer languages |
| **ElevenLabs Scribe v2** | API | ✅ | ✅ | <150ms | 99+ | ✅ | $0.0067/min | Commercial | Ultra-low latency, emotion | Higher cost |
| **Google Chirp 3** | API | ✅ | ✅ | Varies | 70+ | ✅ Likely | $0.006-0.016/min | Commercial | Multilingual, denoiser | Variable pricing |
| **OpenAI GPT-4o-diarize** | API | ❌ | ✅ E2E | - | Multi | ✅ | $0.006/min | Commercial | Smooth transcripts | No streaming, higher WER |
| **Gladia** | API | ✅ | ✅ | <300ms | 100+ | ✅ | $0.0102/min | Commercial | Whisper-quality, code-switch | - |
| **Azure Speech** | API | ✅ | ✅ | Low | 22+ | Check | $0.0042/min | Commercial | Enterprise, Teams integration | - |
| **AWS Transcribe** | API | ✅ | ✅ | Medium | Multi | Check | $0.024/min | Commercial | AWS ecosystem | Higher cost |
| **Qwen3-ASR Flash** | API | ✅ | ❌ | Low | 11 + dialects | ❌ | TBD | Commercial | Context injection, noise robust | No diarization, API only |
| **Soniox** | API | ✅ | ✅ | Low | 60+ | Check | TBD | Commercial | Contact center | - |
| **Rev AI** | API | ✅ | ✅ | Low | Multi | Check | TBD | Commercial | Long-form, transparent | - |
| **Symbl.ai** | API | ✅ | ✅ | Low | Multi | Check | TBD | Commercial | Meeting analytics, insights | Higher cost |
| **IBM Watson STT** | API | ✅ | ✅ | Varies | Multi | Check | TBD | Commercial | Enterprise compliance | - |
| **pyannoteAI** | API | ❌ | ✅ Premium | Fast | Agnostic | ✅ | €99/mo+ | Commercial | Best diarization accuracy | Subscription model |

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Supported/Yes |
| ❌ | Not supported/No |
| Check | Needs verification with vendor |
| OSS | Open-source |
| API | Commercial API |
| RT | Realtime |
| E2E | End-to-end |
| S2S | Speech-to-Speech |
| spk | Speakers |

---

## Quick Reference: Top Picks by Use Case

| Use Case | Recommended | Alternative | Budget Option |
|----------|-------------|-------------|---------------|
| **MVP Production** | AssemblyAI Universal-2 | Deepgram Nova-3 | WhisperX + pyannote |
| **Ultra-low Latency** | ElevenLabs v2 (<150ms) | Deepgram Flux | NeMo Sortformer |
| **Vietnamese Focus** | SenseVoice + pyannote | ElevenLabs | Whisper + VinAI |
| **Privacy/On-prem** | Whisper + Falcon | NeMo + Sortformer | Sherpa-onnx |
| **Mobile/Edge** | Picovoice (Falcon+Leopard) | Sherpa-onnx | - |
| **Long-form English** | Reverb | WhisperX | AssemblyAI |
| **Meeting Analytics** | Symbl.ai | AssemblyAI | Deepgram + LLM |
| **Audio Understanding** | Qwen2-Audio | Voxtral | Ultravox |
| **Enterprise Compliance** | Azure / IBM Watson | pyannoteAI | Google Chirp 3 |

---

## Cost Summary (per hour audio)

| Tier | Solutions | Cost Range |
|------|-----------|------------|
| **Free (self-host)** | WhisperX, pyannote, Diart, SenseVoice | GPU cost only |
| **Budget** | AssemblyAI | ~$0.15/hr |
| **Mid-range** | Deepgram, ElevenLabs, Gladia | ~$0.25-0.40/hr |
| **Premium** | Azure, Google, OpenAI | ~$0.36-0.96/hr |
| **Enterprise** | AWS, Symbl.ai, pyannoteAI | $1.00+/hr |

---

**Note:** Pricing và specs có thể thay đổi. Luôn verify với vendor trước khi quyết định.
