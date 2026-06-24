# 🧠 DopaCheck — 도파민 디톡스 서비스 (팀프로젝트)

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-Web_App-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![GitHub](https://img.shields.io/badge/GitHub-팀_레포지토리-181717?logo=github)](https://github.com/luma-team-ai/DopaCheck/tree/main)

> 배달·SNS·게임 등 도파민 자극 활동을 기록하고, 챌린지를 통해 도파민 디톡스를 실천하는 Flask 웹 서비스.

[← 포트폴리오 목록으로](../README.md) | [팀 전체 레포지토리 →](https://github.com/luma-team-ai/DopaCheck/tree/main)

---

## 팀 구성 및 역할

| 팀원 | 담당 영역 | 레포지토리 |
|---|---|:---:|
| **Nam Heo (본인)** | 홈 모달 UI/UX · 배달 상세 화면 · 보안 개선 | 이 문서 |
| jeongjaebong | 프로젝트 관리 · 문서 · README | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| luma200ok | AI 코치 · OCR 파이프라인 · 챌린지 배치 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| Eun-Seok | 어드민 페이지 · 사용자 관리 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |
| 50seok | 챌린지 기능 · 달성 판정 로직 | [→ 팀 레포](https://github.com/luma-team-ai/DopaCheck/tree/main) |

---

## 내가 담당한 부분

### 1. 홈 모달 UI/UX 개선

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
문제: fetch 실패 시 빈 화면만 표시, 이전 캐시가 잔존해 재시도 시 오류 누적
해결: fetch 에러 발생 시 전체 모달에 에러 메시지 표시
     에러 시 캐시 초기화로 재시도 시 정상 동작 보장
```

---

### 2. 배달 상세 화면 — 아이템 가격 표시 로직 개선

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

---

### 3. 보안 개선 — noise-overlay 외부 URL 제거 (Closes #163)

```
문제: noise-overlay가 외부 AIDA URL을 참조 → 외부 리소스 의존성 + 개인정보 노출 위험
해결: 외부 URL 제거 후 자체 SVG 노이즈 패턴으로 교체
     z-index 99 → 1로 수정 (다른 UI 요소 가림 현상 해소)
```

---

## 담당 파일 목록

| 파일 | 역할 |
|---|---|
| `routes/home.py` | 홈 모달 라우트 (배달 아이템·챌린지 연결) |
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

## 프로젝트 전체 기능 (팀 전체)

> 팀 전체 구현 내용은 **[팀 레포지토리 README](https://github.com/luma-team-ai/DopaCheck/tree/main)** 를 참조하세요.

| 기능 | 담당 |
|---|---|
| 배달 영수증 OCR 분석 (Claude Vision) | luma200ok |
| AI 코치 한마디 (LLM 코멘트) | luma200ok |
| 도파민 챌린지 생성·달성 판정 | 50seok |
| 어드민 대시보드 | Eun-Seok |
| 점수 시스템 | jeongjaebong / luma200ok |
| **홈 모달 UI · 배달 상세 · 보안** | **Nam Heo (본인)** |

---

## 기술 스택

| 영역 | 기술 |
|---|---|
| 언어 | Python 3.11 |
| 백엔드 | Flask |
| 프론트엔드 | Jinja2 · Tailwind CSS |
| AI/OCR | Claude Vision API (Anthropic) |
| DB | MySQL |
| 배포 | Heroku (Procfile) |
| 스케줄러 | APScheduler |
| 테스트 | pytest |

---

*2026 · DopaCheck 팀프로젝트 — 본인 담당 영역 기록*