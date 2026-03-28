## Spec: Backend Payload Capture

**유형**: `API Endpoint`  
**위치**: `Clinet/api/backend-payload-capture.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 실제 백엔드 응답을 받는 즉시 fixture와 테스트 기준으로 빠르게 교체할 수 있도록
> 캡처 위치와 반영 절차를 고정한다.

---

### 대상 파일

#### Auth

- `src/features/auth/api/fixtures/auth-session.json`
- `src/features/auth/api/fixtures/auth-refresh-session.json`

#### Dashboard

- `src/features/dashboard/api/fixtures/metrics-get.response.json`
- `src/features/dashboard/api/fixtures/notifications-get.response.json`
- `src/features/dashboard/api/fixtures/plans-today-get.response.json`
- `src/features/dashboard/api/fixtures/habits-get.response.json`

#### Schedule

- `src/features/schedule/api/fixtures/schedule-board-week.response.json`
- `src/features/schedule/api/fixtures/schedule-create.response.json`
- `src/features/schedule/api/fixtures/schedule-update.response.json`
- `src/features/schedule/api/fixtures/schedule-delete.response.json`

#### Habit

- `src/features/habit/api/fixtures/habit-hub-get.response.json`
- `src/features/habit/api/fixtures/habit-check.response.json`

#### Analytics

- `src/features/analytics/api/fixtures/analytics-overview-get.response.json`
- `src/features/analytics/api/fixtures/analytics-period-compare-get.response.json`

#### Community

- `src/features/community/api/fixtures/community-feed-get.response.json`
- `src/features/community/api/fixtures/community-post-detail-get.response.json`
- `src/features/community/api/fixtures/community-comment-post.response.json`
- `src/features/community/api/fixtures/community-like-post.response.json`
- `src/features/community/api/fixtures/community-report-post.response.json`

#### Curriculum

- `src/features/curriculum/api/fixtures/curriculum-roadmap-get.response.json`
- `src/features/curriculum/api/fixtures/curriculum-roadmap-detail-get.response.json`

#### Shared Error Payloads

- `src/shared/api/fixtures/auth-required.error.json`
- `src/shared/api/fixtures/invalid-credentials.error.json`
- `src/shared/api/fixtures/invalid-period.error.json`
- `src/shared/api/fixtures/refresh-session-invalid.error.json`

---

### 반영 절차

1. 브라우저 Network 탭에서 실제 response payload를 저장한다.
2. 대응되는 fixture JSON을 실제 payload로 교체한다.
3. remote service test와 adapter test를 새 payload 기준으로 갱신한다.
4. nullable field, optional field, validation error, 401 error를 별도 항목으로 정리한다.
5. `npm.cmd test -- --run`과 `npm.cmd run build`를 다시 통과시킨다.

---

### 체크리스트

- [ ] 실제 payload가 spec field와 일치한다.
- [ ] nullable / optional field가 문서에 반영되었다.
- [ ] 401 / validation / empty 응답 예시가 정리되었다.
- [ ] schedule CRUD 응답 구조가 fixture와 테스트에 반영되었다.
- [ ] 테스트와 빌드가 모두 통과한다.
