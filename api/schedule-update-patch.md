## Spec: PATCH /api/v1/schedule/:scheduleId

**유형**: `API`  
**위치**: `docs/specs/api/schedule-update-patch.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 기존 학습 세션의 시간, 상태, 컬럼, 메타 정보를 수정한다.

---

### 엔드포인트

- `PATCH /api/v1/schedule/:scheduleId`

---

### 요청 본문

```json
{
  "view": "week",
  "columnId": "fri",
  "title": "영어 오답 정리 심화",
  "subject": "영어",
  "startTime": "20:10",
  "endTime": "21:00",
  "status": "focus",
  "originLabel": "수동 추가"
}
```

---

### 응답

- 성공 시 최신 schedule board를 `data`로 반환한다.

---

### 검증 규칙

- `scheduleId`는 현재 사용자의 보드에 존재해야 한다.
- 다른 컬럼으로 이동하는 수정도 허용한다.
- drag-and-drop 이동도 이 endpoint를 사용한다.
- 시간과 필수 입력 규칙은 생성 API와 동일하다.

---

### 캡처 Fixture

- `src/features/schedule/api/fixtures/schedule-update.response.json`

---

### 에러

- `400 INVALID_SCHEDULE_INPUT`
- `401 AUTH_REQUIRED`
- `404 SCHEDULE_SESSION_NOT_FOUND`
- `404 SCHEDULE_COLUMN_NOT_FOUND`
