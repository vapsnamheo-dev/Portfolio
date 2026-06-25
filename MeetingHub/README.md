# 📅 MeetingHub — 회의실 예약·회의록 관리 서비스 (팀프로젝트)

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![Next.js](https://img.shields.io/badge/Next.js-000000?logo=next.js&logoColor=white)](https://nextjs.org)
[![GitHub](https://img.shields.io/badge/GitHub-팀_레포지토리-181717?logo=github)](https://github.com/luma200ok/meetinghub)

**회의실 예약부터 회의록 작성·조회·AI 분석까지 — 사내 회의를 한 곳에서 관리하는 풀스택 서비스**

회의실 예약 후 회의록을 바로 작성하고, AI가 자동으로 회의 내용을 분석·요약합니다.

[← 포트폴리오 목록으로](../README.md) · [팀 전체 레포지토리 →](https://github.com/luma200ok/meetinghub)

---

## 📌 프로젝트 정보

|  |  |
|---|---|
| **프로젝트명** | MeetingHub |
| **개발 기간** | 2026.06 |
| **팀 구성** | AI 심화 과정 팀프로젝트 |
| **핵심 개념** | 회의실 예약 → 회의록 작성 → AI 분석 통합 워크플로우 |
| **아키텍처** | Flask 백엔드 REST API + Next.js 14 App Router 프론트엔드 |

---

## ✨ 핵심 기능

| 기능 | 설명 |
|---|---|
| 📋 **회의록 작성** | 예약된 회의에 연결된 회의록 작성, 중복 생성 방지 정책 |
| 🔍 **회의록 조회** | 단건(회의 정보·작성자 함께) / 목록(회의 제목·날짜 함께) |
| ✏️ **회의록 수정** | 작성자 본인 또는 권한자만 수정 가능, 빈 내용 저장 방지 |
| 📅 **회의 상세** | 회의 정보(제목/시간/회의실/참석자) + 회의록 + AI 분석 결과 통합 표시 |
| 🤖 **AI 분석 연동** | 회의록 저장 후 `minute_id` 기반 AI 분석 트리거 |
| 🏢 **기업 격리** | 본인 기업 회의의 회의록만 조회 가능 (크로스 테넌트 차단) |

---

## 팀 구성 및 역할

| 팀원 | 담당 영역 | 레포지토리 |
|---|---|:---:|
| **Nam Heo (본인)** | 회의록 작성 · 회의록 조회 · 회의 상세 | 이 문서 |
| 팀원 | 회의실 예약 · 스케줄 관리 | [→ 팀 레포](https://github.com/luma200ok/meetinghub) |
| 팀원 | AI 분석 모듈 · 자동 요약 | [→ 팀 레포](https://github.com/luma200ok/meetinghub) |
| 팀원 | 사용자 인증 · 권한 관리 | [→ 팀 레포](https://github.com/luma200ok/meetinghub) |

---

## 내가 담당한 부분

### 1. 회의록 작성 (`minute_service.create`)
- `reservation_id`에 연결, `content`(웹 에디터 텍스트), `created_by` 저장
- 한 회의에 회의록 1개 정책 — 중복 생성 방지 로직 구현
- 빈 내용 저장 방지 유효성 검사

### 2. 회의록 조회 (`get`, `list`)

#### 단건 조회
- 회의 정보(예약 정보 join) + 회의록 내용 + 작성자 정보 함께 반환
- `meeting_minutes` ← `meeting_reservations` JOIN 쿼리

#### 목록 조회
- 회의 제목·날짜와 함께 회의록 목록 표시
- 본인 기업 회의의 회의록만 필터링 (기업 격리)

### 3. 회의 상세 화면 (`frontend/src/app/(dashboard)/reservations/[id]/`)
- 회의 정보(제목/시간/회의실/참석자) + 회의록 + AI 분석 결과를 한 페이지에 통합
- AI 분석 트리거 버튼 → `/api/ai/analyze` 호출
- `minute_id`를 AI 분석 엔드포인트에 전달하는 연결 구현

### 4. 회의록 수정 (`update`)
- 작성자 본인 또는 권한자만 수정 가능하도록 권한 통제
- 빈 내용 저장 방지

---

## 담당 파일 목록

| 파일 | 역할 |
|---|---|
| `backend/app/routes/minutes.py` | 회의록 CRUD REST API 라우트 |
| `backend/app/services/minute_service.py` | 회의록 비즈니스 로직 (작성/조회/수정/권한 검사) |
| `frontend/src/app/(dashboard)/minutes/` | 회의록 목록 · 상세 화면 |
| `frontend/src/app/(dashboard)/reservations/[id]/` | 회의 상세 화면 (회의록 진입점, AI 분석 연동) |
| `frontend/src/components/minutes/` | 회의록 에디터 UI 컴포넌트 |

---

## 관련 테이블

| 테이블 | 역할 |
|---|---|
| `meeting_minutes` | 회의록 본문 · 작성자 · 예약 연결 (`reservation_id`) |
| `meeting_reservations` | 회의 예약 정보 — 회의록 조회 시 JOIN |

---

## 🛠️ 기술 스택

| 영역 | 기술 |
|---|---|
| **백엔드** | Python 3.11 · Flask · REST API |
| **프론트엔드** | Next.js 14 (App Router) · TypeScript |
| **DB** | meeting_minutes · meeting_reservations (JOIN) |
| **인증/권한** | JWT 기반 사용자 인증, 기업 격리 |
| **AI 연동** | AI 분석 API 호출 (minute_id 기반) |

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.06 · MeetingHub 팀프로젝트 — 본인 담당 영역 기록*
