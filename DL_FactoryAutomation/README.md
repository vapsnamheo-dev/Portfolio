# DL_FactoryAutomation — 딥러닝 스마트 팩토리

FEMTO 베어링 RUL(잔여 수명) 회귀 + Milling 공구 마모 이진 분류를 딥러닝으로 구현하고 Streamlit 대시보드로 시각화한 프로젝트입니다.

---

## 핵심 결과

| 과제 | 모델 | 최적 OOS RMSE | 비고 |
|---|---|---|---|
| FEMTO RUL 예측 | GRU (batch=16) | **836.6분** | 기준선 956분 대비 12.5% 향상 |
| Milling 공구마모 분류 | 1D-CNN (threshold=0.75) | 학습 중 | 클래스 불균형 처리 |

---

## 모델 구조

### FEMTO — 베어링 잔여 수명 예측 (회귀)

| 모델 | 구조 | 파라미터 |
|---|---|---|
| LSTM | LSTM(64)×2 → Dense(32) → Dense(1) | ~40K |
| GRU ★ | GRU(64)×2 → Dense(32) → Dense(1) | ~30K |

- **입력 텐서**: (batch × timesteps × features) 슬라이딩 윈도우
- **검증**: GroupKFold (베어링 단위 분리 — 시계열 누설 방지)
- **EarlyStopping**: patience=7, 실제 수렴 8-10 에폭

### Milling — 공구 마모 분류 (이진)

| 모델 | 구조 |
|---|---|
| 1D-CNN | Conv1D(64→128→256) → GAP → Dense(128) → Dense(1) |

- **임계값**: 0.75 (클래스 불균형 보정)
- **입력**: 512 timesteps × 6 features

---

## 하이퍼파라미터 튜닝 실험

### 배치 크기 튜닝 (FEMTO GRU)

| batch | OOS RMSE | 비고 |
|---|---|---|
| 16 | **836.6분** ★ | 최적 |
| 32 | 1002.6분 | 기준 재실험 |
| 64 | 1114.4분 | — |
| 128 | 1054.3분 | — |

> **OOS RMSE 각주**: Out-Of-Sample RMSE — 학습 미사용 테스트셋 기준. 기준선 956분은 그리드서치 챔피언(window=20, units=32, dropout=0.1)의 고정 참조값으로, batch=32 재실험값(1002.6분)과 다름. 동일 설정도 시드 변동으로 실행마다 결과 상이.

---

## 권장 HP 튜닝 순서

| 순위 | 항목 | 이유 |
|---|---|---|
| 1 | 배치 크기 | 수렴 품질·속도 직결 |
| 2 | 유닛 수 | 모델 용량 결정 |
| 3 | Dropout | 과적합 방어 |
| 4 | Learning Rate | 수렴 정밀도 |
| 5 | Window 크기 | 시계열 패턴 범위 |
| 6 | 레이어 수 | 깊이 vs 과적합 |
| 7 | EarlyStopping patience | 조기종료 민감도 |
| 8 | Optimizer | Adam으로 충분히 수렴 후 |

---

## 프로젝트 구조



---

## 기술 스택

- **언어**: Python 3.11
- **딥러닝**: TensorFlow 2.21 · Keras
- **모델**: LSTM · GRU · 1D-CNN
- **검증**: GroupKFold · EarlyStopping · ReduceLROnPlateau
- **시각화**: Streamlit · matplotlib · seaborn
- **도구**: numpy · pandas · scikit-learn

---

*소스: C:\AISOURCE\Homework\DL_FactoryAutomation*
