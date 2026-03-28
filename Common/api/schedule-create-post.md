## Spec: POST /api/v1/schedule

**유형**: `API`  
**위치**: `Common/api/schedule-create-post.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 주간 일정 보드에 새 학습 세션을 추가한다.

---

### 엔드포인트

- `POST /api/v1/schedule`

---

### 요청 본문

```json
{
  "view": "week",
  "columnId": "thu",
  "title": "과학 개념 정리",
  "subject": "탐구",
  "startTime": "21:00",
  "endTime": "22:00",
  "status": "planned",
  "originLabel": "수동 추가"
}
```

---

### 응답

- 성공 시 최신 schedule board를 `data`로 반환한다.
- 클라이언트는 반환된 board로 query cache를 갱신한다.

---

### 검증 규칙

- `view`는 현재 `week`만 허용한다.
- `columnId`는 유효한 주간 컬럼이어야 한다.
- `title`, `subject`는 비어 있을 수 없다.
- `endTime`은 `startTime`보다 뒤여야 한다.

---

### 캡처 Fixture

- `src/features/schedule/api/fixtures/schedule-create.response.json`

---

### 에러

- `400 INVALID_SCHEDULE_INPUT`
- `401 AUTH_REQUIRED`
- `404 SCHEDULE_COLUMN_NOT_FOUND`
