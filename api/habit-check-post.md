## Spec: POST /api/v1/habits/check

**유형**: `API`  
**위치**: `docs/specs/api/habit-check-post.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 오늘의 habit 완료 여부를 토글한다.

---

### 엔드포인트

- `POST /api/v1/habits/check`

---

### 요청 본문

```json
{
  "habitId": "habit-review"
}
```

---

### 응답

- 성공 시 최신 habit hub 전체 데이터를 `data`로 반환한다.
- 클라이언트는 반환된 hub 데이터를 그대로 query cache에 반영한다.

---

### 검증 규칙

- `habitId`는 현재 사용자에게 속한 체크리스트 항목이어야 한다.
- 토글 결과에 따라 XP, streak, badge 진행률이 함께 갱신되어야 한다.

---

### 캡처 Fixture

- `src/features/habit/api/fixtures/habit-check.response.json`

---

### 에러

- `401 AUTH_REQUIRED`
- `404 HABIT_NOT_FOUND`
