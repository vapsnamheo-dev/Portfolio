# 🧠 DopaCheck — 도파민 디톡스 서비스 (팀프로젝트)

[![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![Claude](https://img.shields.io/badge/Claude_API-D97757?logo=anthropic&logoColor=white)](https://www.anthropic.com)
[![GitHub](https://img.shields.io/badge/GitHub-팀_레포지토리-181717?logo=github)](https://github.com/luma-team-ai/DopaCheck/tree/main)

**배달 한 번, 스크롤 한 시간 — 내 도파민 소비를 숫자로 마주하는 서비스**

영수증·사용시간을 올리면 AI가 일상 활동·금전 가치로 환산하고, 종합 도파민 점수와 맞춤 챌린지로 소비 패턴을 추적합니다.

🎬 **[시연 영상](https://youtu.be/v-mCKvA6tYQ)** · [← 포트폴리오 목록으로](../README.md) · [팀 전체 레포지토리 →](https://github.com/luma-team-ai/DopaCheck/tree/main)

---

## 📌 프로젝트 정보

|  |  |
|---|---|
| **프로젝트명** | 도파민 체크 (DopaCheck) |
| **개발 기간** | 2026.06.10 ~ 06.17 (7일) |
| **팀 구성** | AI 심화 과정 6인 팀 |
| **핵심 개념** | 배달·디지털 소비 → 일상 활동·금전 가치 환산 → 도파민 점수화 |
| **배포** | Cloudtype (`main` push 자동 배포) |

---

## 🎬 데모 — 히스토리 UI

> 내가 담당한 **히스토리 기능** 화면입니다. 이미지 클릭 시 전체 시연 영상(YouTube)으로 이동합니다.

[![히스토리 UI](https://raw.githubusercontent.com/luma-team-ai/DopaCheck/main/docs/shots/flow-08-history.png)](https://youtu.be/v-mCKvA6tYQ)

---

## ✨ 핵심 기능

| 기능 | 설명 |
|---|---|
| 🍗 **배달 분석** | 영수증 사진 업로드 → AI(OCR) 자동 추출 → 지출·칼로리를 일상 활동으로 환산 + 공감 코멘트 |
| ⏰ **시간 분석** | 앱별 사용 시간 입력 → 대체 활동(책·강의·운동)·시급 기준 기회비용 환산 + 이번 주 누적 추적 |
| 📊 **종합 리포트** | 배달·시간·점수 통합 대시보드 + 주간 비교 차트 |
| 🔥 **도파민 점수** | 0~100 점수 산출, 전체 평균 대비 / 상위 N% 랭킹 |
| 🏆 **AI 챌린지** | 내 히스토리 기반 맞춤 챌린지 추천, 달성 시 점수 감점(=개선) |
| 📋 **히스토리** ⭐ | 배달·시간 기록 통합 조회, 주차별 활동 이력 + XSS 방어 처리 |
| 👤 **소셜 로그인** | Google · Kakao OAuth |

---

## 팀 구성 및 역할

| 팀원 | 담당 영역 | 레포지토리 |
|---|---|:---:|
| **Nam Heo (본인)** | 히스토리 기능 · 홈 모달 UI/UX · 배달 상세 · 보안 개선 | 이 문서 |
| 정재봉 | 오케스트레이션·설계·머지·배포 · 초기 스캐폴딩 · 종합 리포트 · 공통 인프라(MariaDB 전환) | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| 김승현 | 소셜 로그인(Google/Kakao) · DB 총괄 · 홈 대시보드 · 마이페이지 · 점수 트렌드 차트 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| 오영석 | AI 모듈(추천·생성) · 챌린지(달성 판정·동시성) · OCR 프롬프트 개선 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| 이은석 | 배포(Procfile·헬스체크) · 관리자 페이지 · 시간 분석(/time) · Stitch UI | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| 김관영 | 배달 영수증 분석 라우트 · CSRF DRY 통합 · 세션 보안 · 모바일 대응 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |

---

## 내가 담당한 부분

### 1. 히스토리 기능
- 배달·시간 기록 통합 조회 페이지 구현 (`routes/history.py`)
- 주차별 활동 이력 표시 및 시간분석·점수·랭킹 연동

### 2. 홈 모달 UI/UX 개선

#### 배달 아이템 상세 이동 + 챌린지 버튼 동작 수정
- 홈 모달에서 배달 아이템 클릭 시 상세 페이지로 이동 연결
- 챌린지 버튼 클릭 시 챌린지 페이지로 올바르게 라우팅

#### XSS 보안 방어 — badgeHtml 분리 (Closes #185)
```
문제: badgeHtml에 외부 문자열을 innerHTML로 직접 삽입 → XSS 취약점
해결: badgeStyle을 Tailwind 허용 목록 클래스로 교체
     badgeHtml을 createElement+textContent로 분리해 DOM 안전 삽입
```

#### fetch 에러 UX 개선 + 캐시 초기화 (Closes #195)
```
문제: fetch 실패 시 빈 화면만 표시, 이전 캐시 잔존으로 재시도 시 오류 누적
해결: fetch 에러 발생 시 전체 모달에 에러 메시지 표시
     에러 시 캐시 초기화로 재시도 정상 동작 보장
```

### 3. 배달 상세 화면 — 아이템 가격 표시 로직 개선

#### 0원 처리 → 총액-배달비 균등 분배 표시 (Closes #190)
```
문제: OCR이 개별 아이템 가격을 0원으로 인식한 경우 모든 항목이 0원 표시
해결: 가격이 모두 0원인 경우 총액에서 배달비를 제한 금액을 아이템 수로 균등 분배 표시
```

#### 균등 분배 제거 → 음식 합계 표시 + 혼합 케이스 처리 (Closes #191)
```
문제: 일부 아이템만 0원인 혼합 케이스에서 균등 분배 로직이 잘못 적용
해결: 균등 분배 제거, 인식된 개별 가격 합계를 음식 합계로 표시
     혼합 케이스(일부 0원/일부 금액 있음) 각각 정확하게 처리
```

### 4. 보안 개선 — noise-overlay 외부 URL 제거 (Closes #163)

```
문제: noise-overlay가 외부 AIDA URL 참조 → 외부 리소스 의존성 + 개인정보 노출 위험
해결: 외부 URL 제거 후 자체 SVG 노이즈 패턴으로 교체
     z-index 99 → 1로 수정 (다른 UI 요소 가림 현상 해소)
```

---

## 담당 파일 목록

| 파일 | 역할 |
|---|---|
| `routes/history.py` | 히스토리 라우트 (배달·시간 기록 통합 조회) |
| `routes/home.py` | 홈 모달 라우트 (배달 아이템·챌린지 연결) |
| `templates/history/` | 히스토리 템플릿 |
| `templates/home/` | 홈 모달 템플릿 (badge XSS 방어·에러 UX) |
| `routes/delivery.py` | 배달 상세 라우트 |
| `templates/delivery/` | 배달 상세 템플릿 (아이템 가격 표시) |
| `static/` | noise-overlay SVG (외부 URL → 자체 SVG) |

---

## 기여 PR 목록

| PR | 내용 | 종류 |
|---|---|:---:|
| #187 | noise-overlay 외부 AIDA URL 제거 + z-index 수정 | 보안 |
| #190 | 배달 상세 0원 아이템 총액-배달비 균등 표시 | 버그 수정 |
| #192 | 배달 상세 균등 분배 제거 → 음식 합계 표시 + 혼합 케이스 처리 | 버그 수정 |
| #193 | 홈 모달 badgeHtml XSS 방어 + fetch 에러 전체 모달 표시 | 보안/UX |
| #196 | 홈 모달 badgeStyle 허용목록 교체 + fetch 에러 캐시 초기화 | 보안/버그 수정 |

---

## 기술 스택

| 영역 | 기술 |
|---|---|
| **백엔드** | Python 3.10 · Flask · Jinja2 · Gunicorn |
| **데이터** | MariaDB (커넥션 풀, `user_id` 격리) |
| **AI** | Claude API (OCR · 칼로리 추론 · 공감 코멘트 · 점수 · 챌린지 추천) |
| **인증** | OAuth 2.0 (Google · Kakao) |
| **프론트** | Tailwind CSS (PostCSS 빌드) · Chart.js |
| **배포** | Cloudtype (`main` push 자동 배포) |

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.06 · DopaCheck 팀프로젝트 — 본인 담당 영역 기록*