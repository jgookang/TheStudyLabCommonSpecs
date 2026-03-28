## Spec: Endpoint Priority

**유형**: `API`  
**위치**: `docs/specs/api/endpoint-priority.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> 실제 `/api/v1` 전환 전에 어떤 endpoint를 먼저 검증해야 하는지 우선순위를 정리한다.

---

### P0. 즉시 검증 필요

- [x] `POST /api/v1/auth/login`
- [x] `POST /api/v1/auth/refresh`
- [x] `POST /api/v1/auth/logout`
- [x] `GET /api/v1/dashboard/metrics`
- [x] `GET /api/v1/dashboard/plans/today`
- [x] `GET /api/v1/dashboard/notifications`

### P1. 현재 화면 연결 완료

- [x] `GET /api/v1/dashboard/habits`
- [x] `GET /api/v1/curriculum/roadmaps`
- [x] `GET /api/v1/curriculum/roadmaps/:roadmapId`
- [x] `GET /api/v1/schedule/board?view=week|month`
- [x] `POST /api/v1/schedule`
- [x] `PATCH /api/v1/schedule/:scheduleId`
- [x] `DELETE /api/v1/schedule/:scheduleId`
- [x] `GET /api/v1/habits`
- [x] `POST /api/v1/habits/check`
- [x] `GET /api/v1/analytics/overview`
- [x] `GET /api/v1/analytics/compare`
- [x] `GET /api/v1/community/feed`
- [x] `GET /api/v1/community/posts/:postId`
- [x] `POST /api/v1/community/posts/:postId/comments`
- [x] `POST /api/v1/community/posts/:postId/like`
- [x] `POST /api/v1/community/posts/:postId/report`

### P2. 다음 기능 확장

- [ ] premium / billing 관련 endpoint

---

### 메모

- query key는 endpoint spec 이름과 같은 축으로 맞춘다.
- remote service는 실제 endpoint 호출 계약을 테스트로 고정한다.
- 실제 백엔드 payload를 받으면 fixture와 spec을 바로 갱신한다.
