# 🏗️ HajaCheck — AI 기반 시설물 외관 하자 점검 플랫폼 (팀프로젝트)

[![Java](https://img.shields.io/badge/Java-17-007396?logo=openjdk&logoColor=white)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3.5-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![React](https://img.shields.io/badge/React-18.3-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![GitHub](https://img.shields.io/badge/GitHub-팀_레포지토리-181717?logo=github)](https://github.com/luma-team-ai/HajaCheck)

**사진을 올리면 하자를 찾고, 등급을 매기고, 보고서 초안까지 씁니다.**

시설물(건물 외벽·교량 등) 사진을 업로드하면 유형별 전용 Vision 모델(균열 U-Net · 면적형 YOLO)이 균열·박리박락·철근노출을 탐지하고, 규칙 기반으로 A~E 등급을 매긴 뒤 LLM(LangChain + RAG)이 점검 보고서 초안과 법규 질의응답까지 지원합니다.

🚀 **[라이브 데모](https://hajacheck.luma200ok.com)** · [← 포트폴리오 목록으로](../README.md) · [팀 전체 레포지토리 →](https://github.com/luma-team-ai/HajaCheck)

---

## 📌 프로젝트 정보

|  |  |
|---|---|
| **프로젝트명** | HajaCheck (하자체크) |
| **개발 기간** | 2026.07.09 ~ 08.07 (4주) |
| **팀 구성** | AI 심화 과정 8인 팀 — 메뉴 담당제(전원 화면+API+AI 연동 직접 구현) |
| **핵심 개념** | 사진 업로드 → AI 하자 탐지 → 등급 산정 → 휴먼 검수 → LLM 보고서 초안 |
| **배포** | OCI(자체 서버) Docker Compose + nginx — main 승격 시 CD 자동배포 |

---

## 🎬 데모 — 대시보드

> 내가 주담당으로 개발한 **대시보드 · 시설물 관리** 화면입니다.

[![대시보드](https://raw.githubusercontent.com/luma-team-ai/HajaCheck/main/docs/shots/dashboard.png)](https://hajacheck.luma200ok.com)

---

## ✨ 핵심 기능

| 기능 | 설명 |
|---|---|
| 📤 **사진 업로드·분석** | 청크 업로드 + 매직바이트(파일 시그니처) 검증 → Vision 모델(균열 U-Net · YOLO) 하자 탐지 |
| 🏷️ **등급 산정** | 탐지 결과 규칙 기반 A~E 등급 산정 + 휴먼 검수 |
| 📊 **대시보드** | 처리 대기 현황·최근 점검·하자 등급 분포 요약 |
| 🏢 **시설물 관리** | 시설물 CRUD, 점검 이력 타임라인, 회차 간 비교(사진·KPI·PDF 내보내기), 점검 주기 설정·도래 알림 |
| 🔔 **알림 센터** | 점검 도래·검수 대기 등 이벤트 알림 + AI 주간 브리핑 |
| 📝 **AI 보고서** | LangChain + RAG 기반 점검 보고서 초안 자동 생성 |
| 💬 **AI 어시스턴트** | RAG 챗봇 기반 법규 질의응답 |

---

## 팀 구성 및 역할

> AI 심화 과정 8인 팀 · 2026.07.09 ~ 08.07 · 메뉴 담당제(전원 화면+API+AI 연동 직접 구현)

| 팀원 | 담당 영역 | 레포지토리 |
|---|---|:---:|
| **Nam Heo (본인)** | 대시보드 · 시설물 관리 개발 · 미디어 파이프라인 오너(청크 업로드·매직바이트 검증) | 이 문서 |
| 김승현 (PM) | PRD·아키텍처 설계, 일정·산출물 관리 · 관리자 페이지 개발 · RAG(검색) 코치 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 정재봉 (PL) | 로그인·회원가입(OCR) 개발 · DevOps 리드(OCI 인프라·CI/CD) · 공용 DB·배포 운영 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 유병현 | DB 스키마 기초 설계 · 하자 관리 + 통계 개발 · 데이터(DB·API 계약) 오너 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 오영석 | 점검 관리 B(결과 뷰어·검수) · 검수 API + AI 하자 설명 · LLM(생성) 코치 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 이은석 | 고객지원 개발·실시간(상담) 오너 · RAG 챗봇 파이프라인 · STOMP 서버·인증·대기열 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 김관영 | 랜딩 + 보고서 + 지도 뷰 개발 · AI 풀스택(LLM 보고서 생성 체인) · 보고서 편집·PDF 화면 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |
| 황승현 | 점검 관리 A(업로드·분석) · AI 추론(DL) 오너 · 추론 API 연동·비동기 잡·폴링 규약 | [→ 팀 레포](https://github.com/luma-team-ai/HajaCheck) |

---

## 내가 담당한 부분

### 1. 대시보드 · 시설물 관리 (주담당)
- 현황 요약(처리 대기 현황) 대시보드 개발
- 시설물 CRUD, 점검 이력 타임라인 · 회차 간 변화 비교 화면 개발
- 점검 주기 설정 · 도래 표시, 알림 센터 + AI 주간 브리핑 직접 구현

### 2. 점검 관리 A (부담당) — 업로드·분석
- 업로드/프레임 추출 연동, 추론 API 연동·비동기 잡 직접 구현(AI-DL 담당자와 협업)

### 3. 미디어 파이프라인 오너
- 청크 업로드, 매직바이트(파일 시그니처) 검증 설계·구현

---

## 트러블슈팅 (일부)

| 이슈 | 내용 | 관련 PR |
|---|---|:---:|
| 하자 상세 AI 설명 패널 공란 | 존재하지 않는 엔드포인트 호출 + 필수값 검증 실패 + 응답 스키마 불일치(mock이 버그를 가림)를 진단·수정 | #1364 |
| 하자 위치 마커 고정좌표 표시 | 정적 SVG 마킹 대신 실제 AI 탐지 bbox 좌표 기반 오버레이로 교체 | #1370 |
| 회차 간 비교 PDF 내보내기 실패 | Tailwind v4가 생성하는 `oklab` 색상 함수를 PDF 캡처 라이브러리(html2canvas)가 파싱하지 못해 항상 실패하던 것을 근본 원인까지 추적해 수정 | #1606 |
| PDF 내보내기 시 드롭다운 글자 겹침 | html2canvas가 네이티브 `<select>`를 캡처하지 못하는 구조적 한계를 캡처 직전 텍스트로 치환하는 방식으로 해결 | #1614 |
| 점검 이력 사진 미리보기 게이팅 결함 | "사진 표시 여부"와 "+N 버튼 표시 여부" 조건이 하나로 묶여있던 결함을 분리 | #1576 |
| 시설물 등록 시 주소 미입력 허용 | 프론트·백엔드 양쪽 모두 주소 필수 검증이 누락돼 있던 것을 발견·수정 | #1561 |

---

## 기여 PR 목록 (일부)

| PR | 내용 | 종류 |
|---|---|:---:|
| #1123 | 점검 알림 중복 방지 — 애플리케이션 체크 → DB 유니크 제약 전환 | 기능 |
| #1238 | 시설물 카드 하자건수 배지 — 배치 조회 API 신설 | 기능 |
| #1289 | 시설물 대표사진 선택 기능 | 기능 |
| #1521 | 점검 이력 결과보기/보고서 딥링크 연결 | 기능 |
| #1364 | 하자 상세 AI 설명 패널 공란 수정 | 버그 수정 |
| #1370 | 하자 위치 마커 고정좌표 표시 수정 | 버그 수정 |
| #1561 | 시설물 등록 주소 필수 검증 추가 | 버그 수정 |
| #1606 | 회차 간 비교 PDF 내보내기 실패(oklab 파싱) 수정 | 버그 수정 |
| #1614 | PDF 내보내기 드롭다운 글자 겹침 수정 | 버그 수정 |

---

## 기술 스택

| 영역 | 기술 |
|---|---|
| **백엔드** | Java 17 · Spring Boot 3.3.5 · Gradle · JPA · QueryDSL |
| **프론트** | React 18 · Vite · TypeScript · Tailwind CSS · TanStack Query |
| **AI 서버** | Python · FastAPI · LangChain · Vision(U-Net · YOLO) · RAG(Chroma) |
| **데이터** | PostgreSQL 16 |
| **인프라** | OCI(자체 서버) Docker Compose + nginx · GitHub Actions CI + PR머신 자동 검수 · main 승격 CD |

---

## 📬 Contact

- GitHub: [@vapsnamheo-dev](https://github.com/vapsnamheo-dev)
- Email: vapsnamheo@gmail.com

---

*2026.08 · HajaCheck 팀프로젝트 — 본인 담당 영역 기록*
