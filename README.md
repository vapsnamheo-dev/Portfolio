# Nam Heo | Portfolio

머신러닝·딥러닝·데이터 분석·웹앱 개발 프로젝트를 정리한 포트폴리오입니다.

- 📧 vapsnamheo@gmail.com
- 🐙 [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)

---

## 프로젝트 목록

| # | 프로젝트 | 설명 | 기술 스택 | 구분 | 데모 |
|:---:|---|---|---|:---:|:---:|
| 1 | [PdM-Guard — 설비 고장 예측](./ML_FactoryAutomation/README.md) | 설비 센서 데이터로 고장 확률 예측 · XGBoost(ROC-AUC 0.97, T*=0.75 임계값 최적화 시 정밀도 0.93/재현율 0.81) + Gemini AI 해설 | Python · XGBoost · Streamlit | 개인프로젝트 | [🔗 Live](https://mlfactoryautomation.streamlit.app/) |
| 2 | [DopaCheck — 도파민 디톡스](./DopaCheck/README.md) | 배달·SNS·게임 활동 기록 + AI 코치 · 담당: 홈 UI/UX · 보안 개선 | Python · Flask · Claude Vision · MySQL | 팀프로젝트 | [🔗 Live](https://dopacheck.luma200ok.com/home) |
| 3 | [MeetingHub — 회의실·회의록 관리](./MeetingHub/README.md) | 회의실 예약 + 회의록 작성·조회 + AI 분석 · 담당: 회의록 CRUD | Python · Flask · Next.js | 팀프로젝트 | — |
| 4 | [realtime-translator — 실시간 통역](./realtime-translator/README.md) | 시스템 오디오 → STT → Groq 번역·답변 · PyQt6 반투명 오버레이 | Python · faster-whisper · Groq · PyQt6 | 개인프로젝트 | — |
| 5 | [DL_FactoryAutomation — 딥러닝 스마트팩토리](./DL_FactoryAutomation/README.md) | FEMTO 베어링 RUL 예측(GRU+BN+LN, OOS RMSE 810.4분 · v1 대비 16.75% 개선) + Autoencoder 이상탐지(AUC 0.968) + Milling 공구마모 분류(1D-CNN) · Streamlit 대시보드 | Python · TensorFlow · Keras · Streamlit | 개인프로젝트 | [🔗 Live](https://dlfactoryautomation.streamlit.app/) |
| 6 | [LLM_FactoryAutomation — LLM 기반 예지보전 진단](./LLM_FactoryAutomation/README.md) | ML 열화 분류(AUC 0.99) + DL RUL 예측(GRU) + RAG(FAISS·Chroma Hybrid) 검색 결과를 이중 LLM(Claude API+로컬 Ollama)으로 자연어 정비 보고서 자동 생성 | Python · Anthropic Claude API · Ollama · FAISS · ChromaDB · Streamlit | 개인프로젝트 | [🔗 Live](https://llmfactoryautomation.streamlit.app/) |
| 7 | [HajaCheck — AI 시설물 하자 점검](./HajaCheck/README.md) | 사진 업로드 → AI 하자 탐지·등급 산정 → LLM 보고서 초안 · 담당: 대시보드·시설물 관리 개발 + 미디어 파이프라인 오너 | Java · Spring Boot · React · PostgreSQL · LangChain | 팀프로젝트 | [🔗 Live](https://hajacheck.luma200ok.com) |

---

## 기술 역량

| 영역 | 기술 |
|---|---|
| 딥러닝 | TensorFlow · Keras · LSTM · GRU · BiLSTM · 1D-CNN · EarlyStopping · GroupKFold |
| ML / AI | scikit-learn · XGBoost · SHAP · Anthropic Claude API · Google Gemini API · Groq API |
| LLM / RAG | Claude API(Structured Output) · Ollama(로컬 LLM) · FAISS · ChromaDB · Hybrid RAG(BM25+벡터) |
| 데이터 | pandas · numpy · matplotlib · seaborn · plotly |
| 웹 백엔드 | Flask · Next.js · SQLAlchemy · REST API |
| 웹앱 | Streamlit |
| 데스크톱 | PyQt6 · WASAPI 루프백 · faster-whisper |
| DB | SQLite · MySQL · PostgreSQL |

---

*마지막 업데이트: 2026.07*
