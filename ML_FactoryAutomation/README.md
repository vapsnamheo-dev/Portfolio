# 🛠️ ML_FactoryAutomation — 설비 고장 예측 시스템 (PdM-Guard)

[![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-Live_Demo-FF4B4B?logo=streamlit&logoColor=white)](https://mlfactoryautomation.streamlit.app/)
[![XGBoost](https://img.shields.io/badge/XGBoost-ROC--AUC_0.97-2ECC71)](https://xgboost.readthedocs.io)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org)
[![License](https://img.shields.io/badge/Data-AI4I_2020_CC_BY_4.0-lightgrey)](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)

> 설비 센서 데이터(온도·회전속도·토크·공구마모)로 **고장 발생을 사전에 예측**하는 예지보전(PdM) 머신러닝 시스템.  
> XGBoost 모델이 실시간으로 고장 확률을 산출하고, Gemini AI가 원인·조치를 해설합니다.

[← 포트폴리오 목록으로](../README.md)

---

## 목차

1. [애플리케이션 화면](#1-애플리케이션-화면)
2. [데모 영상](#2-데모-영상)
3. [자동 생성 보고서 샘플](#3-자동-생성-보고서-샘플)
4. [주요 기능](#4-주요-기능)
5. [실행 방법](#5-실행-방법)
6. [파일 업로드 형식](#6-파일-업로드-형식)
7. [트러블슈팅](#7-트러블슈팅)
8. [아키텍처](#8-아키텍처)
9. [보고서 생성 기준](#9-보고서-생성-기준)
10. [LLM 설정](#10-llm-설정)
11. [데이터 입출력 인터페이스](#11-데이터-입출력-인터페이스)
12. [고장 유형 레이블](#12-고장-유형-레이블)
13. [모델링 실험 기록](#13-모델링-실험-기록)
14. [프로젝트 구조](#14-프로젝트-구조)
15. [알려진 제약사항](#15-알려진-제약사항)
16. [데이터 출처](#16-데이터-출처)
17. [라이선스](#17-라이선스)

---

## 1. 애플리케이션 화면

| 단건 예측 탭 | CSV 일괄 검증 탭 | 성능 대시보드 탭 |
|:---:|:---:|:---:|
| ![단건예측](./assets/screen_predict.png) | ![일괄검증](./assets/screen_batch.png) | ![대시보드](./assets/screen_dashboard.png) |

> 스크린샷은 `assets/` 폴더에 업로드 후 표시됩니다.

---

## 2. 데모 영상

<!-- 녹화된 mp4 파일을 아래 경로에 업로드하세요: ML_FactoryAutomation/assets/demo.mp4 -->

> **영상 추가 방법**
> 1. GitHub 이슈 또는 PR 댓글 창에 mp4 파일을 드래그&드롭
> 2. 생성된 URL(`https://github.com/…/assets/…mp4`)을 위 링크로 교체
> 3. 또는 `ML_FactoryAutomation/assets/demo.mp4` 로 직접 업로드 후 아래 태그 사용:
>
> ```html
> <video src="./assets/demo.mp4" controls width="100%"></video>
> ```

---

## 3. 자동 생성 보고서 샘플

앱 내에서 두 가지 보고서를 자동 생성·다운로드할 수 있습니다.

| 보고서 | 생성 위치 | 형식 | 내용 |
|---|---|:---:|---|
| **성능 대시보드 보고서** | 성능 대시보드 탭 하단 "📥 대시보드 결과 저장" | `.md` | 3모델 성능 비교표 + 고장유형 파레토 데이터 |
| **AI 분석·조치 히스토리** | CSV 일괄 검증 탭 하단 "💾 히스토리 저장" | `.md` | 검증 요약 + Gemini AI 해설 + Q&A 챗봇 기록 |

**보고서 샘플 (텍스트)**
```
# PdM-Guard Performance Dashboard Report
- Generated: 2026-06-23 06:05
- Decision Threshold T*: 0.85

## Model Performance Comparison
| Model          | Accuracy | Precision | Recall | F1     | ROC-AUC |
|----------------|----------|-----------|--------|--------|---------|
| XGBoost        | 0.9790   | 0.6548    | 0.8088 | 0.7237 | 0.9706  |
| RandomForest   | 0.9830   | 0.8269    | 0.6324 | 0.7167 | 0.9611  |
| LogisticReg    | 0.8250   | 0.1421    | 0.8235 | 0.2424 | 0.9069  |
```

---

## 4. 주요 기능

### 🔮 탭 1 — 단건 예측
- 사이드바 슬라이더로 센서값(공기온도·공정온도·회전속도·토크·공구마모) 입력
- XGBoost가 고장 확률(%) 즉시 산출
- **물리규칙(AI4I) 실제 라벨**과 예측 결과 일치 여부 비교 표시
- 예측 이력 SQLite DB 자동 저장 및 최근 10건 조회
- 사이드바 임계값 슬라이더(0.0~1.0) + 변경 이력 로그

### 📁 탭 2 — CSV 일괄 검증
- CSV 파일 업로드 또는 레포 샘플 데이터 선택
- **전처리 가드**: 필수 컬럼 누락 · 문자열 공백 · 센서 결측값 자동 정리
- 혼동행렬(TP/TN/FP/FN) 및 정확도·고장탐지율·오경보 수 시각화
- 불일치(오답) 건만 별도 필터링 표시
- **Gemini AI 자동 해설** + **Q&A 챗봇** (원인·조치 문답)
- AI 분석 히스토리 `.md` 다운로드

### 📊 탭 3 — 성능 대시보드 (4개 서브탭)

| 서브탭 | 내용 |
|---|---|
| 🎯 모델 성능 요약 | 3모델 KPI 카드 · 정확도 도넛 차트 · 혼동행렬 6지표 비교표 · 자동 해설 |
| 🔥 고장유형 파레토 | TWF/HDF/PWF/OSF 빈도 파레토 차트 · 80% 점유 유형 자동 식별 |
| 🔬 상관·분포 분석 | 피처 상관관계 히트맵 · 정상 vs 고장 박스플롯 · 고장 상관 상위 피처 |
| 📌 특성 중요도 | XGBoost Feature Importance 수평 막대 차트 · 상위 3대 피처 해설 |

---

## 5. 실행 방법

### 로컬 실행

```bash
# 1. 가상환경 생성 및 활성화
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate    # macOS/Linux

# 2. 의존성 설치
pip install -r requirements.txt

# 3. 모델 학습 (최초 1회)
python -m src.train

# 4. 앱 실행
streamlit run app/streamlit_app.py
```

브라우저에서 `http://localhost:8501` 접속

### Streamlit Community Cloud 배포

1. GitHub 저장소 push
2. [share.streamlit.io](https://share.streamlit.io) → New app
3. Repository: `vapsnamheo-dev/AISOURCE` / Branch: `main`
4. Main file path: `Homework/ML_FactoryAutomation/app/streamlit_app.py`
5. Secrets에 `GEMINI_API_KEY` 등록 (선택)
6. Deploy

---

## 6. 파일 업로드 형식

### 필수 컬럼

| 컬럼명 | 타입 | 예시 | 설명 |
|---|:---:|---|---|
| `Type` | str | `L`, `M`, `H` | 제품 등급 |
| `Air temperature [K]` | float | `298.1` | 공기 온도 |
| `Process temperature [K]` | float | `308.6` | 공정 온도 |
| `Rotational speed [rpm]` | int | `1551` | 회전속도 |
| `Torque [Nm]` | float | `42.8` | 토크 |
| `Tool wear [min]` | int | `108` | 공구 마모 시간 |

### 선택 컬럼

| 컬럼명 | 타입 | 설명 |
|---|:---:|---|
| `Target` | int (0/1) | 실제 고장 여부 (있으면 실제 vs 예측 비교 활성화) |

### 샘플 CSV

```csv
Type,Air temperature [K],Process temperature [K],Rotational speed [rpm],Torque [Nm],Tool wear [min],Target
M,298.1,308.6,1551,42.8,108,0
L,303.5,313.8,1408,63.2,201,1
H,299.3,309.1,1600,35.0,50,0
```

> 전처리 가드가 앞뒤 공백·결측값을 자동 정리합니다.

---

## 7. 트러블슈팅

| 증상 | 원인 | 해결 |
|---|---|---|
| `ModuleNotFoundError: src` | 실행 경로 불일치 | 프로젝트 루트에서 `streamlit run app/streamlit_app.py` 실행 |
| 모델 파일 없음 오류 | 학습 전 앱 실행 | `python -m src.train` 먼저 실행 |
| CSV 업로드 후 "필수 컬럼 누락" | 컬럼명 불일치 | [파일 업로드 형식](#6-파일-업로드-형식) 참조 |
| AI 해설이 규칙 기반으로 표시 | GEMINI_API_KEY 미설정 | 환경변수 또는 `.streamlit/secrets.toml`에 키 등록 |
| 차트 한글 깨짐 (로컬) | 시스템 폰트 없음 | matplotlib `font.family`를 로컬 한글 폰트로 변경 |
| Streamlit Cloud 배포 후 DB 초기화 | SQLite 파일 비영속 | 환경변수 `DATABASE_URL`로 PostgreSQL 연결 |
| 포트 충돌 (`Port 8501 is already in use`) | 기존 프로세스 점유 | `netstat -ano \| findstr 8501` 후 해당 PID 종료 |

---

## 8. 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                   Streamlit Web App                      │
│  ┌──────────────┐ ┌──────────────┐ ┌─────────────────┐  │
│  │ 🔮 단건 예측  │ │📁 CSV 일괄검증│ │ 📊 성능 대시보드 │  │
│  └──────┬───────┘ └──────┬───────┘ └────────┬────────┘  │
│         │                │                  │            │
│  ┌──────▼───────────────▼──────────────────▼──────────┐ │
│  │              src/ 모듈 계층                          │ │
│  │  predict.py │ model_store.py │ preprocess.py       │ │
│  │  evaluate.py │ config.py │ synth_ai4i.py           │ │
│  └──────┬──────────────────────────────┬──────────────┘ │
│         │                              │                  │
│  ┌──────▼──────┐              ┌────────▼──────────┐      │
│  │   ML 모델   │              │  Gemini AI API    │      │
│  │  XGBoost    │              │  (해설·챗봇)       │      │
│  │  RandomForest│             └───────────────────┘      │
│  │  LogisticReg│                                          │
│  └──────┬──────┘                                          │
│         │                                                  │
│  ┌──────▼──────────────────────┐                          │
│  │  SQLAlchemy DB              │                          │
│  │  SQLite(로컬) / PostgreSQL  │                          │
│  │  prediction / model_registry│                          │
└──┴─────────────────────────────┴──────────────────────────┘
```

**데이터 흐름**
```
센서 입력 → preprocess.py(전처리·검증) → predict.py(XGBoost 추론)
→ db.py(이력 저장) → Streamlit UI(결과 표시) → Gemini AI(해설)
```

---

## 9. 보고서 생성 기준

### 성능 대시보드 보고서 (`dashboard_YYYYMMDD_HHMM.md`)

| 섹션 | 생성 기준 |
|---|---|
| Model Performance | `model/model_info.json` 로드 · 3모델 accuracy/precision/recall/f1/roc_auc |
| Failure Type Pareto | 학습 데이터 `Failure Type` 컬럼 집계 · 고장 건수 내림차순 |
| 생성 시점 | 버튼 클릭 시각 (`datetime.now()`) |
| 운영 임계값 | `config.DECISION_THRESHOLD` (기본 0.75, F1 최적) |

### AI 분석 히스토리 (`pdm_ai_history.md`)

| 항목 | 내용 |
|---|---|
| 검증 요약 | 총 건수·정확도·혼동행렬(TP/FP/FN/TN)·놓친 고장 평균 센서값 |
| AI 해설 | Gemini 1.5 Flash 자동 분석 (3~5줄 + 핵심 조치) |
| Q&A 기록 | 사용자 질문 + Gemini 답변 (최근 5건) |

---

## 10. LLM 설정

**방법 1 — 환경변수 (로컬)**
```bash
# Windows PowerShell
$env:GEMINI_API_KEY = "your-api-key-here"
streamlit run app/streamlit_app.py
```

**방법 2 — secrets.toml (로컬)**
```toml
# .streamlit/secrets.toml
GEMINI_API_KEY = "your-api-key-here"
```

**방법 3 — Streamlit Cloud Secrets**
```
share.streamlit.io → 앱 설정 → Secrets
GEMINI_API_KEY = "your-api-key-here"
```

### 폴백 동작

| 상태 | 동작 |
|---|---|
| API 키 설정됨 | Gemini 1.5 Flash 호출 → AI 해설·챗봇 활성화 |
| API 키 없음 | 규칙 기반 요약 자동 폴백 (FN·FP 조치 가이드) |
| API 오류(쿼터·네트워크) | 오류 메시지 표시 후 규칙 기반으로 전환 |

---

## 11. 데이터 입출력 인터페이스

### 입력

| 인터페이스 | 형식 | 설명 |
|---|:---:|---|
| 사이드바 슬라이더 | UI | 단건 예측용 센서값 직접 입력 |
| CSV 업로드 | `.csv` | 일괄 검증용 다건 입력 |
| 레포 샘플 로드 | 내부 경로 | `demo data/demo_1000.csv` · `predictive_maintenance.csv` |

### 출력

| 인터페이스 | 형식 | 설명 |
|---|:---:|---|
| 예측 이력 DB | SQLite / PostgreSQL | `prediction` 테이블에 건별 저장 |
| 성능 보고서 | `.md` | 대시보드 결과 다운로드 |
| AI 히스토리 | `.md` | AI 해설·Q&A 기록 다운로드 |
| 대시보드 차트 | Matplotlib PNG | Streamlit 인라인 렌더링 |

### DB 스키마 (주요 테이블)

```sql
-- 예측 이력
CREATE TABLE prediction (
    prediction_id   INTEGER PRIMARY KEY,
    model_id        INTEGER,
    pred_proba      REAL,
    pred_label      INTEGER,
    predicted_at    DATETIME
);

-- 모델 레지스트리
CREATE TABLE model_registry (
    id          INTEGER PRIMARY KEY,
    name        TEXT,
    version     TEXT,
    is_active   BOOLEAN,
    created_at  DATETIME
);
```

---

## 12. 고장 유형 레이블

| 코드 | 고장 유형 | 물리 조건 | 빈도 (10K 기준) |
|:---:|---|---|:---:|
| **TWF** | 공구 마모 고장 | 공구 마모 ≥ 200~240 min (랜덤 임계) | ~46건 |
| **HDF** | 방열 고장 | 공기·공정 온도 차 < 8.6 K AND rpm < 1380 | ~115건 |
| **PWF** | 전력 부족 고장 | 전력(토크×rpm) < 3500 W 또는 > 9000 W | ~95건 |
| **OSF** | 과부하 고장 | 토크 × 공구마모 > 제품등급별 임계값 | ~98건 |
| **RNF** | 임의 고장 | 0.1% 확률 랜덤 발생 | ~19건 |

**클래스 불균형**: 정상 96.6% / 고장 3.4% → `scale_pos_weight=28.5` 적용

---

## 13. 모델링 실험 기록

### 모델 비교 (3종)

| 모델 | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|:---:|:---:|:---:|:---:|:---:|
| **XGBoost** ⭐ | 0.9790 | 0.6548 | 0.8088 | 0.7237 | **0.9706** |
| RandomForest | 0.9830 | 0.8269 | 0.6324 | 0.7167 | 0.9611 |
| LogisticRegression | 0.8250 | 0.1421 | 0.8235 | 0.2424 | 0.9069 |

XGBoost 채택 이유: ROC-AUC 최고 + 불균형 데이터 `scale_pos_weight` 지원

### 임계값 최적화

기본 0.5 대신 **PR 곡선 기반 F1 최적 임계값 T\*=0.75** 적용:
- 정밀도 0.833 → 0.932 (오경보 감소)
- 재현율 0.809 유지
- 앱 사이드바 슬라이더로 운영자가 실시간 조정 가능

### 실험 리포트 (`reports/` 폴더)

| 리포트 | 내용 |
|---|---|
| `classifier_compare.csv` | 3모델 성능 비교 |
| `lasso_coef.csv` / `lasso_perf.csv` | Lasso 정규화 계수·성능 |
| `learning_curve.csv` / `lc_big.csv` | 데이터 크기별 학습 곡선 |
| `overfit_gap.csv` | Train/Test 갭 분석 |
| `tuning_comparison.csv` | 하이퍼파라미터 튜닝 비교 |
| `outlier_report.csv` / `outlier_effect.csv` | 이상치 탐지·영향 분석 |
| `cmp_small.csv` / `cmp_big.csv` / `cmp_real10k.csv` | 합성·실제 데이터 분포 비교 |

---

## 14. 프로젝트 구조

```
ML_FactoryAutomation/
├── app/
│   └── streamlit_app.py        # Streamlit 웹앱 (3탭 + 4서브탭)
├── src/
│   ├── config.py               # 경로·하이퍼파라미터·환경변수 단일 출처
│   ├── data_loader.py          # CSV 로드·컬럼 검증
│   ├── preprocess.py           # 전처리 가드 (결측·공백·인코딩·스케일)
│   ├── train.py                # 3모델 학습 (LogReg·RF·XGBoost)
│   ├── evaluate.py             # 성능 지표·혼동행렬·비교
│   ├── predict.py              # 추론 파이프라인 + DB 로깅
│   ├── db.py                   # SQLAlchemy 모델·세션·초기화
│   ├── model_store.py          # DB 모델 레지스트리·일괄 예측
│   ├── synth_ai4i.py           # 물리규칙 합성 데이터 생성 (최대 100K)
│   ├── lasso.py                # Lasso 정규화 계수 분석
│   ├── vif_analysis.py         # VIF 다중공선성 분석
│   ├── compare_classifiers.py  # 분류기 성능 비교
│   ├── compare_datasets.py     # 실제·합성 데이터 분포 비교
│   └── bigdata.py              # 대용량 데이터 실험
├── model/
│   ├── xgb_model.pkl           # XGBoost 학습 완료 모델
│   ├── rf_model.pkl            # RandomForest 학습 완료 모델
│   ├── logreg_model.pkl        # LogisticRegression 학습 완료 모델
│   ├── scaler.pkl              # StandardScaler
│   ├── model_info.json         # 하이퍼파라미터·성능 지표 기록
│   └── model_card.md           # 모델 카드 (재현용 문서)
├── reports/                    # 실험 결과 CSV (20+ 리포트)
├── notebooks/
│   └── 홍길동_머신러닝프로젝트.ipynb  # 학습·탐색용 Jupyter 노트북
├── demo data/
│   └── demo_1000.csv           # 시연용 샘플 (1,000건)
├── docs/
│   └── DEPLOYMENT_NOTE.md      # 배포 가이드
├── CHANGES.md                  # 변경 이력
├── .streamlit/
│   ├── config.toml             # 테마 설정
│   └── secrets.toml.example    # API 키 예시
└── requirements.txt
```

---

## 15. 알려진 제약사항

| 항목 | 제약 | 대안 |
|---|---|---|
| **DB 영속성** | Streamlit Cloud는 SQLite 파일 재시작 시 초기화 | 환경변수 `DATABASE_URL`로 Supabase/PostgreSQL 연결 |
| **Gemini 쿼터** | 무료 API 키는 분당 요청 제한 있음 | 유료 키 사용 또는 규칙 기반 폴백 활용 |
| **예측 대상** | AI4I 물리규칙 기반 데이터 전용 | 실제 설비 데이터로 재학습 필요 |
| **다중 사용자** | Streamlit 세션 상태는 사용자별 독립, DB만 공유 | 운영 환경에서는 사용자 인증 추가 권장 |
| **차트 폰트** | Cloud 환경 한글 폰트 미지원 → 영문 레이블 사용 | 커스텀 폰트 파일 배포 후 `rcParams` 설정 |

---

## 16. 데이터 출처

- **데이터셋**: [AI4I 2020 Predictive Maintenance Dataset](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)
- **출처**: UCI Machine Learning Repository
- **라이선스**: CC BY 4.0
- **크기**: 10,000행 × 14컬럼
- **논문**: Matzka, S. (2020). *Explainable Artificial Intelligence for Predictive Maintenance Applications*

---

## 17. 라이선스

- **데이터**: AI4I 2020 Predictive Maintenance Dataset — [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **코드**: 교육·포트폴리오 목적 (비상업적 사용)

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.06 · ML_FactoryAutomation (PdM-Guard) v1.0*