## Spec: DELETE /api/v1/schedule/:scheduleId

**유형**: `API`  
**위치**: `Common/api/schedule-delete-delete.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 주간 일정 보드에서 특정 학습 세션을 삭제한다.

---

### 엔드포인트

- `DELETE /api/v1/schedule/:scheduleId?view=week`

---

### Query

| key | type | required | note |
| --- | --- | --- | --- |
| `view` | `'week'` | yes | 현재는 주간 CRUD만 지원 |

---

### 응답

- 성공 시 최신 schedule board를 `data`로 반환한다.

---

### 검증 규칙

- `scheduleId`는 현재 사용자의 보드에 존재해야 한다.
- 월간 summary 세션은 직접 삭제 대상이 아니다.

---

### 캡처 Fixture

- `src/features/schedule/api/fixtures/schedule-delete.response.json`

---

### 에러

- `401 AUTH_REQUIRED`
- `404 SCHEDULE_SESSION_NOT_FOUND`
