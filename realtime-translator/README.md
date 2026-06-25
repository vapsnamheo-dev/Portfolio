# 🎙️ realtime-translator — 실시간 회의 통역 오버레이 (개인프로젝트)

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![PyQt6](https://img.shields.io/badge/PyQt6-41CD52?logo=qt&logoColor=white)](https://pypi.org/project/PyQt6/)
[![Groq](https://img.shields.io/badge/Groq_API-F55036?logoColor=white)](https://console.groq.com)
[![GitHub](https://img.shields.io/badge/GitHub-소스_레포지토리-181717?logo=github)](https://github.com/vapsnamheo-dev/AISOURCE/tree/main/Homework/realtime-translator)

**Zoom / Google Meet 상대방 목소리를 실시간 캡처 → 한국어 자막 + 영어 답변 3개 자동 추천**

시스템 오디오를 로컬 STT로 즉시 인식하고, Groq 무료 API로 번역·답변을 추천하는 **반투명·항상 위·나만 보이는** 데스크톱 오버레이입니다. 모든 처리가 무료이며 외부 유료 서비스 없이 동작합니다.

[← 포트폴리오 목록으로](../README.md) · [소스 레포지토리 →](https://github.com/vapsnamheo-dev/AISOURCE/tree/main/Homework/realtime-translator)

---

## 📌 프로젝트 정보

|  |  |
|---|---|
| **프로젝트명** | 실시간 회의 통역 오버레이 (meet-overlay) |
| **개발 기간** | 2026.06 (개인 사이드 프로젝트) |
| **구분** | 개인프로젝트 |
| **핵심 개념** | WASAPI 루프백 → faster-whisper STT → Groq LLM → PyQt6 오버레이 실시간 파이프라인 |
| **실행 환경** | Windows 10/11 (WASAPI 루프백 캡처 필요) |

---

## ✨ 핵심 기능

| 기능 | 설명 |
|---|---|
| 🎤 **실시간 STT** | WASAPI 루프백으로 시스템 오디오 캡처 → `faster-whisper` 로컬 모델로 영어 인식 |
| 🌐 **실시간 번역** | Groq API (`llama-3.3-70b-versatile`)로 영→한 즉시 번역, 오버레이 왼쪽에 표시 |
| 💬 **답변 추천** | `Ctrl+Space` 또는 버튼 → 최근 대화 컨텍스트 기반 영어 답변 3개 + 한국어 설명 |
| 🪟 **오버레이 UI** | PyQt6 반투명 창 (항상 위, 알파 0.85), 화면 상단 고정, Zoom/Meet 위에 표시 |
| ⚡ **저지연 설계** | `base` 모델 기준 ~1~2초 내 자막, GPU 있으면 ~1초 이내 |
| 🔒 **완전 무료** | STT = 로컬 faster-whisper, 번역/답변 = Groq 무료 API (유료 과금 없음) |

---

## 🏗️ 아키텍처

```
시스템 오디오 (WASAPI 루프백)
        ↓
audio_capture.py  ── 16kHz mono PCM 스트림
        ↓
stt.py            ── faster-whisper 부분/확정 자막
        ↓
llm.py            ── Groq API (번역 + 답변 추천)
        ↓
overlay.py        ── PyQt6 반투명 오버레이 표시
```

---

## 📁 담당 파일 목록

| 파일 | 역할 |
|---|---|
| `main.py` | 진입점 · 스레드 파이프라인 연결 |
| `audio_capture.py` | WASAPI 루프백(시스템 오디오) 캡처 → 16kHz mono |
| `stt.py` | faster-whisper 스트리밍 STT (부분/확정 자막) |
| `llm.py` | Groq API — 영→한 번역 + 영어 답변 추천 3개 |
| `overlay.py` | PyQt6 반투명 오버레이 UI (항상 위, 핫키 처리) |
| `config.py` | 설정값 관리 (.env 로드, Whisper 모델·지연시간 등) |
| `.env.example` | 환경변수 템플릿 (GROQ_API_KEY 등) |

---

## 🛠️ 기술 스택

| 영역 | 기술 |
|---|---|
| **오디오 캡처** | PyAudioWPatch (WASAPI 루프백, Windows 전용) |
| **음성 인식(STT)** | faster-whisper (로컬, `base`/`small` 모델) |
| **번역 / 답변 추천** | Groq API (`llama-3.3-70b-versatile` / `llama-3.1-8b-instant`) |
| **UI** | PyQt6 (반투명 오버레이, 항상 위, 전역 핫키) |
| **설정 관리** | python-dotenv |
| **언어** | Python 3.11 |

---

## 🚀 설치 및 실행

```powershell
# 1. 의존성 설치
cd realtime-translator
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt

# 2. Groq 무료 API 키 설정 (https://console.groq.com/keys 에서 발급)
cp .env.example .env
# .env 파일 열어 GROQ_API_KEY=gsk_... 입력

# 3. 실행
python main.py
# Ctrl+Space → 영어 답변 추천
```

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.06 · 개인 사이드 프로젝트*
