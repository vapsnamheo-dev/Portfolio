# 🛠️ PdM-Guard — 설비 고장 예측 시스템 (개인프로젝트)

[![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-Live_Demo-FF4B4B?logo=streamlit&logoColor=white)](https://mlfactoryautomation.streamlit.app/)
[![XGBoost](https://img.shields.io/badge/XGBoost-ROC--AUC_0.97-2ECC71)](https://xgboost.readthedocs.io)
[![GitHub](https://img.shields.io/badge/GitHub-소스코드-181717?logo=github)](https://github.com/vapsnamheo-dev/AISOURCE)

> 설비 센서 데이터(온도·회전속도·토크·공구마모)로 **고장 발생을 사전에 예측**하는 예지보전(PdM) 머신러닝 시스템.
> XGBoost 모델이 실시간으로 고장 확률을 산출하고, Gemini AI가 원인·조치를 해설합니다.

### 🔗 [라이브 데모 →](https://mlfactoryautomation.streamlit.app/)

[← 포트폴리오 목록으로](../README.md)

---

### 📊 모델 성능

| 모델 | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|:---:|:---:|:---:|:---:|:---:|
| **XGBoost** ⭐ | **97.9%** | 65.5% | **80.9%** | **72.4%** | **0.9706** |
| RandomForest | 98.3% | 82.7% | 63.2% | 71.7% | 0.9611 |
| LogisticRegression | 82.5% | 14.2% | 82.4% | 24.2% | 0.9069 |

> **운영 임계값 T*=0.85** 적용 — PR 곡선 F1 최적점. 오경보↓ · 재현율 유지.
> 고장 탐지에서 **재현율(Recall) 우선**: 고장을 놓치는 비용 > 오경보 비용이므로 XGBoost 선택.

---

### 🖥️ 주요 기능

| 탭 | 기능 |
|---|---|
| **🔮 단건 예측** | 사이드바 슬라이더로 센서값 입력 → 고장 확률 즉시 예측 + 물리규칙 실제값 비교 + 예측 이력 DB 저장 |
| **📁 CSV 일괄 검증** | CSV 업로드 → 전처리 가드(결측·공백 자동 정리) → 혼동행렬(TP/FP/FN/TN) + Gemini AI 해설·챗봇 |
| **📊 성능 대시보드** | 3모델 비교 · 고장유형 파레토 · 상관관계 히트맵 · XGBoost 특성 중요도 |

---

### ⚙️ 기술 스택

| 영역 | 기술 |
|---|---|
| 언어 | Python 3.10 |
| ML | scikit-learn · XGBoost · SHAP |
| 데이터 | pandas · numpy |
| 시각화 | matplotlib · seaborn |
| 웹앱 | Streamlit |
| AI 어시스턴트 | Google Gemini 1.5 Flash (미설정 시 규칙 기반 폴백) |
| DB | SQLAlchemy + SQLite(로컬) / PostgreSQL(클라우드) |
| 배포 | Streamlit Community Cloud · GitHub Actions CI |

---

### 🗂️ 프로젝트 구조

```
ML_FactoryAutomation/
├── app/streamlit_app.py       # Streamlit 예측 웹앱 (3탭 구성)
├── src/
│   ├── config.py              # 경로·하이퍼파라미터·환경변수
│   ├── train.py               # 모델 학습 (LogReg · RF · XGBoost)
│   ├── evaluate.py            # 성능 지표·비교·SHAP
│   ├── predict.py             # 추론 + DB 로깅
│   ├── preprocess.py          # 전처리 가드
│   ├── db.py                  # SQLAlchemy 세션·스키마
│   ├── model_store.py         # DB 모델 레지스트리
│   └── synth_ai4i.py          # 물리규칙 합성 데이터 생성 (최대 100K)
├── model/
│   ├── xgb_model.pkl          # XGBoost (ROC-AUC 0.9706)
│   ├── rf_model.pkl           # RandomForest
│   └── model_card.md          # 모델 카드
└── data/predictive_maintenance.csv   # AI4I 2020 (10,000행)
```

---

### 🔍 핵심 설계 결정

| 결정 | 이유 |
|---|---|
| XGBoost 최종 선택 | ROC-AUC 0.97 최고 성능 + scale_pos_weight로 불균형(3.4%) 보정 |
| 운영 임계값 T*=0.85 | PR 곡선 F1 최적점 — 정밀도 0.655→0.932, 재현율 유지 |
| 물리규칙 합성 데이터 | AI4I 10K 부족 시 동일 물리규칙으로 최대 100K 확장 가능 |
| Gemini API 폴백 설계 | API 키 없을 때 규칙 기반 요약으로 자동 폴백 — 오프라인도 동작 |
| SQLite → PostgreSQL 전환 | 환경변수 DATABASE_URL 하나로 로컬↔클라우드 전환 |

---

### 📁 데이터셋

- **출처**: [AI4I 2020 Predictive Maintenance Dataset](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset) — UCI ML Repository (CC BY 4.0)
- **크기**: 10,000행 × 14컬럼 / 피처 11개
- **고장 유형**: TWF(공구마모) · HDF(방열) · PWF(전력부족) · OSF(과부하) · RNF(임의)
- **클래스 비율**: 정상 96.6% / 고장 3.4% (불균형)

---

### 🚀 로컬 실행

```bash
python -m venv .venv && .venv\Scripts\activate
pip install -r requirements.txt
python -m src.train
streamlit run app/streamlit_app.py
```

> Gemini AI 기능 활성화: 환경변수 `GEMINI_API_KEY` 설정 필요

---

*2026.06 · PdM-Guard v1.0*