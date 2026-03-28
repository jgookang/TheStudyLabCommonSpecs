## Spec: Backend Phase 1 Plan

**유형**: `Feature`  
**위치**: `Server/features/backend-phase-1-plan.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> `TheStudyLabServer` 저장소에서 실제 서버 구현을 시작하기 전에
> 우선순위, 선행 조건, 설계 축, 검증 기준을 고정한다.

---

### 입력 기준 문서

- [session-handoff.md](/C:/Users/jgook/repo/CommonSpecs/Server/features/session-handoff.md)
- [phase-1-next-steps.md](/C:/Users/jgook/repo/CommonSpecs/Server/features/phase-1-next-steps.md)
- [auth-session-lifecycle.md](/C:/Users/jgook/repo/CommonSpecs/Server/backend/auth-session-lifecycle.md)
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\metrics-get.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\plans-today-get.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\notifications-get.md`

---

### 현재 상태 정리

- 현재 저장소에는 서버 구현 코드가 없고, 문서와 계획만 존재한다.
- 공용 API 계약은 `CommonSpecs` 에서 관리한다.
- 웹 auth 는 same-site + cookie refresh + refresh always `401` 기준으로 고정한다.
- 모바일 auth 는 같은 인증 코어를 쓰되 token transport 만 분리한다.
- dashboard P0 read spec 은 구현 전 기준 문서로 사용할 수 있도록 `Ready` 상태로 정리한다.
- 프론트 handoff 기준으로 가장 시급한 목표는 새 기능 추가가 아니라 실제 web backend smoke 통과다.

---

### Phase 1 최우선 목표

1. 웹 프론트가 실제 백엔드로 로그인, 세션 복구, 로그아웃을 수행할 수 있어야 한다.
2. 대시보드 P0 조회 endpoint 가 실제 응답을 반환해야 한다.
3. 하나의 인증 코어 위에 모바일 auth transport 를 추가할 수 있어야 한다.
4. smoke 결과를 바탕으로 fixture, adapter, spec 예시를 동기화할 수 있어야 한다.

---

### 구현 범위

#### P0. 서버 기초

- 프로젝트 부트스트랩
- 환경 변수 구조
- 공통 에러 응답 형식
- 인증 미들웨어
- DB migration 기본 구조
- seed 또는 smoke 테스트용 계정 준비

#### P0. 웹 인증

- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `POST /api/v1/auth/web/logout`
- same-site cookie / CORS / credentials / Origin 검증 정렬

#### P0. 대시보드 read

- `GET /api/v1/dashboard/metrics`
- `GET /api/v1/dashboard/plans/today`
- `GET /api/v1/dashboard/notifications`

#### P1. 모바일 인증 transport

- `POST /api/v1/auth/mobile/login`
- `POST /api/v1/auth/mobile/refresh`
- `POST /api/v1/auth/mobile/logout`

#### P1. 핵심 read / mutation

- curriculum roadmap 조회
- schedule board 조회 및 CRUD
- habit hub 조회 및 check mutation
- analytics overview / compare
- community feed / detail / comment / like / report

---

### 권장 구현 순서

#### Step 0. 서버 골격 확정

- 런타임, 웹 프레임워크, ORM 또는 query layer, migration 도구를 확정한다.
- health check 와 공통 error envelope 를 먼저 고정한다.
- `AUTH_REQUIRED`, `REFRESH_SESSION_NOT_FOUND`, `REFRESH_SESSION_INVALID` 등 공통 에러 코드를 정리한다.

#### Step 1. 공통 인증 코어 구현

- session 저장 모델, token rotation, revoke 흐름을 먼저 구현한다.
- `clientType='web' | 'mobile'` 기준으로 공통 세션 레코드를 설계한다.

#### Step 2. 웹 auth 흐름 구현 및 smoke

- [auth-session-lifecycle.md](/C:/Users/jgook/repo/CommonSpecs/Server/backend/auth-session-lifecycle.md) 기준으로 same-site web auth 를 먼저 완성한다.
- 프론트 remote smoke 의 첫 통과 기준을 web auth 로 잡는다.

#### Step 3. 대시보드 P0 응답 구현

- metrics / plans / notifications 는 프론트 첫 화면 안정화에 직결된다.
- 이 세 endpoint 는 `Ready` 공용 스펙을 기준으로 read model 중심으로 구현한다.

#### Step 4. 모바일 auth transport 추가

- 공통 인증 코어 위에 mobile login / refresh / logout transport 를 얹는다.
- secure storage 기반 refresh rotation 계약을 서버에서 고정한다.

#### Step 5. 일정 / 습관 핵심 mutation 구현

- `schedule create / update / delete`
- `habit check`
- mutation 성공 시 프론트가 최신 보드 또는 hub 전체를 갱신할 수 있게 응답을 구성한다.

#### Step 6. 나머지 read endpoint 확장

- curriculum
- analytics
- community

#### Step 7. 실응답 동기화

- 실제 payload 를 fixture 에 반영한다.
- 공용 스펙 예시를 갱신한다.
- smoke test 결과 문서를 남긴다.

---

### 작업 스트림별 설계 초점

#### 1. 인증 / 세션

- refresh token 저장소
- access token 검증 방식
- logout 멱등성
- web Origin 검증
- 모바일 device session 관리

#### 2. 대시보드 read model

- metrics 집계 기준 기간
- plans 의 사용자 timezone 기준
- notifications 의 `createdAt` 및 표시용 상대 시간 처리 기준

#### 3. 일정 / 습관 / 커뮤니티 mutation

- write 후 최신 read model 을 바로 반환할지 여부
- transaction 경계
- 중복 요청과 idempotency 처리

#### 4. 관측성과 운영

- auth / mutation 실패 로그
- request tracing
- smoke 테스트 계정 및 seed 관리

---

### 현재 확인된 리스크와 빈칸

- 프론트 웹 저장소는 현재 단일 `/api/v1/auth/*` 경로를 사용하므로 `/api/v1/auth/web/*` 로 맞추는 변경이 필요하다.
- 모바일 앱 저장소가 아직 없거나 미확정이면 mobile auth contract 는 server-first 로 검증해야 한다.
- plans 의 timezone 기준과 notifications 의 상대 시간 렌더링 책임은 실제 구현 전에 클라이언트와 다시 확인해야 한다.
- 현재 저장소에는 구현 코드가 없으므로, 실제 작업 시작 전 부트스트랩 범위를 작게 잡아야 한다.

---

### Definition of Done

- web auth smoke 가 성공한다.
- dashboard P0 read endpoint 가 실제 응답을 반환한다.
- mobile auth transport 계약이 서버 구현 기준으로 닫힌다.
- 최소 1개 이상의 schedule 또는 habit mutation 이 프론트 연동 기준으로 검증된다.
- 공용 스펙과 실제 응답 차이가 문서에 반영된다.

---

### 바로 다음 행동

1. auth/session 설계를 기준으로 서버 골격을 부트스트랩한다.
2. web auth endpoint 3개부터 구현하고 smoke 를 맞춘다.
3. dashboard P0 read 모델의 실제 응답 shape 를 공용 스펙 기준으로 맞춘다.
4. mobile auth transport 를 공통 세션 코어 위에 추가한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 서버 Phase 1 구현 계획 문서 추가 | Codex |
| 2026-03-28 | 웹/모바일 분리형 auth 와 Ready P0 기준으로 계획 갱신 | Codex |
