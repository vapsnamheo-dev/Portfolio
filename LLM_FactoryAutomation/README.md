# LLM_FactoryAutomation — LLM 기반 예지보전 진단 시스템

FEMTO-ST 베어링 PdM에 Claude AI(LLM)를 통합하여 ML/DL/RAG 수치 예측 결과를 현장 담당자가 즉시 이해할 수 있는 **자연어 정비 권고 보고서**로 자동 생성하는 시스템입니다.

DL_FactoryAutomation의 ML+DL+RAG 기반 위에 Proposal A(LLM 보고서 생성)를 구현하였습니다.

---

## 핵심 기능 — Proposal A

```
센서 측정값
    |
    +-- ML 모델 (RF/XGB) ---- 열화 확률 + 정상/열화 판정
    +-- DL 모델 (GRU/LSTM) -- 잔여수명(RUL) 예측 (분)
    +-- RAG (FAISS 인덱스) -- 유사 사례 Top-3 검색
             |
    Claude API (claude-haiku-4-5)
             |
    자연어 진단 보고서 (4섹션)
    [현재상태] [이상신호] [정비권고] [유사사례]
```

---

## 시스템 구성

| 레이어 | 모듈 | 역할 |
|---|---|---|
| LLM | `src/femto_llm_report.py` | Claude API 호출, 자연어 보고서 생성 |
| RAG | `src/femto_rag_search.py` | FAISS 인덱스, 유사 사례 Top-k 검색 |
| ML | `src/femto_ml.py` | RF/XGB 열화 분류 (AUC 0.99) |
| DL | `src/femto_dl_rul.py` | GRU/LSTM RUL 회귀 (OOS RMSE 836.6분) |
| UI | `app/streamlit_femto.py` | 6탭 대시보드 (Tab6: AI 정비 권고) |

---

## Tab6 — AI 정비 권고

Streamlit Tab6에서 원클릭으로 AI 보고서를 생성합니다:

1. 센서 슬라이더 입력 (h_rms, h_kurt, v_rms 등 8개 + 온도)
2. "AI 정비 권고 보고서 생성" 버튼 클릭
3. ML 확률 / DL RUL / RAG 유사사례 중간 메트릭 표시
4. Claude API 자연어 4섹션 보고서 출력

**ANTHROPIC_API_KEY 미설정 시**: 규칙 기반 Mock 모드로 자동 전환 (API 비용 없이 시연 가능)

---

## 빠른 시작

```bash
# 1. 환경 설정
pip install -r requirements.txt

# 2. API 키 설정 (선택 -- 없으면 Mock 모드)
set ANTHROPIC_API_KEY=sk-ant-...

# 3. 데이터 전처리 (최초 1회)
python -m src.femto_preprocess

# 4. 모델 학습
python -m src.femto_ml
python -m src.femto_dl_rul
python -m src.femto_rag_search   # FAISS 인덱스 빌드

# 5. Streamlit 실행
streamlit run app/streamlit_femto.py
```

---

## 모델 성능

| 모델 | 유형 | 성능 |
|---|---|---|
| RandomForest | ML 열화 분류 | AUC 0.99, Recall 0.91 |
| GRU+BN+LN | DL RUL 예측 | OOS RMSE 836.6분 |
| Autoencoder | 비지도 이상탐지 | AUC 0.968 (라벨 불필요) |
| FAISS RAG | 유사 사례 검색 | 코사인 유사도, 12-dim |

---

## 기술 스택

- **언어**: Python 3.11
- **LLM**: Anthropic Claude API (claude-haiku-4-5-20251001)
- **딥러닝**: TensorFlow 2.x · Keras (GRU, LSTM, Autoencoder)
- **ML**: scikit-learn (RandomForest, XGBoost)
- **RAG**: FAISS (IndexFlatIP, 코사인 유사도)
- **UI**: Streamlit
- **설명가능성**: SHAP (LinearExplainer, KernelExplainer)

---

## 프로젝트 구조

```
LLM_FactoryAutomation/
├── src/
│   ├── femto_llm_report.py    # Claude API + 자연어 보고서 생성
│   ├── femto_rag_search.py    # FAISS 유사 사례 검색
│   ├── femto_ml.py            # ML 열화 분류
│   ├── femto_dl_rul.py        # DL RUL 예측
│   └── femto_preprocess.py    # 데이터 전처리
├── app/
│   └── streamlit_femto.py     # 6탭 대시보드 (Tab6: AI 정비 권고)
├── 산출물/
│   └── LLM-RAG 프로젝트/
│       ├── LLM_산출물_20260629_FEMTO_LLM진단.docx
│       └── LLM_프로젝트_발표용.pptx
└── requirements.txt
```

---

*데이터: FEMTO-ST PRONOSTIA IEEE PHM 2012 베어링 가속열화 데이터셋*
