# 🛠️ PdM-Guard — 설비 고장 예측 ML 프로젝트

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)](https://streamlit.io)
[![XGBoost](https://img.shields.io/badge/XGBoost-0.9.2+-FF6600)](https://xgboost.readthedocs.io)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org)

**센서 데이터 5개로 설비 고장 확률을 예측하는 예지보전(PdM) 머신러닝 프로젝트**

AI4I 공개 데이터셋을 기반으로 XGBoost·RandomForest·LogisticRegression 3종 모델을 비교하고, 최적 임계값 조정과 Gemini AI 어시스턴트를 포함한 Streamlit 웹앱으로 시연합니다.

[← 포트폴리오 목록으로](../README.md)

---

## 📌 프로젝트 정보

|  |  |
|---|---|
| **프로젝트명** | PdM-Guard (Predictive Maintenance Guard) |
| **개발 기간** | 2026.06 (개인 과제) |
| **데이터셋** | AI4I 2020 Predictive Maintenance Dataset (UCI) |
| **핵심 목표** | 5개 센서값 → 고장 확률 예측 + 임계값 최적화 + 운영 시뮬레이션 |

---

## ✨ 주요 기능

| 탭 | 기능 |
|---|---|
| 🔮 **단건 예측** | 사이드바 슬라이더로 센서값 입력 → 고장 확률·판정 + 물리 규칙 실제값 대조 |
| 📁 **CSV 일괄 검증** | CSV 업로드 또는 레포 샘플 → 일괄 예측 → 혼동행렬·정확도·불일치 상세 |
| 📊 **성능 대시보드** | 모델 성능 요약 / 고장유형 파레토 / 상관·분포 분석 / 특성 중요도 |
| 🤖 **AI 어시스턴트** | Gemini API 연동 자동 해설 + 조치 Q&A 챗봇 (키 없을 시 규칙 기반 폴백) |

---

## 🔬 모델링 과정

### 데이터 & 특성

| 입력 특성 | 설명 |
|---|---|
| `Type` | 제품 등급 (L·M·H) |
| `Air temperature [K]` | 공기 온도 (295~305 K) |
| `Process temperature [K]` | 공정 온도 (305~314 K) |
| `Rotational speed [rpm]` | 회전속도 |
| `Torque [Nm]` | 토크 |
| `Tool wear [min]` | 공구 마모 시간 |

**고장 유형(다중 라벨)**: TWF(공구마모) · HDF(방열) · PWF(전력부족) · OSF(과부하)

### 모델 비교 (3종)

| 모델 | 특징 |
|---|---|
| **XGBoost** ⭐ 최종 채택 | 불균형 데이터 `scale_pos_weight` 적용, F1 최고 |
| RandomForest | 앙상블 기반, Feature Importance 해석 |
| Logistic Regression | 베이스라인, Lasso 정규화 계수 분석 |

### 임계값 최적화

기본 0.5가 아닌 **PR 곡선 기반 F1 최적 임계값 T\*=0.75** 적용:
- 정밀도 ↑ (오경보 감소)
- 재현율 0.809 유지
- 앱에서 실시간 슬라이더로 운영자가 조정 가능

### 주요 실험 리포트

`reports/` 폴더에 CSV 형태로 저장:

| 리포트 | 내용 |
|---|---|
| `classifier_compare.csv` | 3모델 성능 비교 |
| `lasso_coef.csv` | Lasso 회귀 계수 (특성 중요도) |
| `learning_curve.csv` | 데이터 크기별 학습 곡선 |
| `overfit_gap.csv` | Train/Test 갭 분석 |
| `tuning_comparison.csv` | 하이퍼파라미터 튜닝 비교 |

---

## 🗂️ 프로젝트 구조

```
ML_FactoryAutomation/
├── app/
│   └── streamlit_app.py        # Streamlit 웹앱 (단건·일괄·대시보드·AI)
├── src/
│   ├── train.py                # 모델 학습
│   ├── predict.py              # 예측 + DB 로깅
│   ├── evaluate.py             # 평가 지표
│   ├── model_store.py          # DB 모델 관리
│   ├── preprocess.py           # 전처리 가드
│   ├── compare_classifiers.py  # 3모델 비교
│   ├── lasso.py                # Lasso 분석
│   ├── synth_ai4i.py           # 물리 규칙 라벨 생성
│   ├── db.py                   # SQLAlchemy DB
│   └── config.py               # 설정 (임계값·경로)
├── reports/                    # 실험 결과 CSV
├── model/                      # 학습된 모델 + model_info.json
└── demo data/
    └── demo_1000.csv           # 시연용 샘플
```

---

## 🖥️ 실행 방법

```bash
# 의존성 설치
pip install -r requirements.txt

# Streamlit 앱 실행
streamlit run app/streamlit_app.py

# (선택) Gemini AI 어시스턴트 활성화
export GEMINI_API_KEY=your_key_here
```

---

## 기술 스택

| 영역 | 기술 |
|---|---|
| **ML** | XGBoost · scikit-learn · Lasso · RandomForest |
| **데이터** | pandas · numpy · AI4I Dataset (UCI) |
| **시각화** | matplotlib · seaborn · Streamlit |
| **DB** | SQLAlchemy (SQLite, 예측 이력 저장) |
| **AI 연동** | Google Gemini API (`gemini-1.5-flash`) |
| **웹앱** | Streamlit (3탭 + 4서브탭 구성) |

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.06 · 개인 ML 과제 — 설비 예지보전(PdM) 프로토타입*