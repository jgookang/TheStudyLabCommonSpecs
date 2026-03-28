## Spec: GET /api/v1/schedule/board

**유형**: `API`  
**위치**: `docs/specs/api/schedule-board-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 일정 화면에서 주간 또는 월간 보드를 조회한다.

---

### 엔드포인트

- `GET /api/v1/schedule/board?view=week|month`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### Query

| key | type | required | note |
| --- | --- | --- | --- |
| `view` | `'week' \| 'month'` | yes | 보드 기준 단위 |

---

### 응답

```json
{
  "data": {
    "view": "week",
    "rangeLabel": "3월 4주차",
    "overview": {
      "totalSessions": 3,
      "completedSessions": 1,
      "focusHours": 4.5,
      "autoPlannedCount": 1
    },
    "columns": [
      {
        "id": "wed",
        "label": "수요일",
        "subLabel": "03.26",
        "isCurrent": true,
        "sessions": [
          {
            "id": "week-wed-1",
            "title": "수학 개념 복습",
            "subject": "수학",
            "startTime": "19:00",
            "endTime": "20:10",
            "status": "completed",
            "originLabel": "자동 추천"
          }
        ]
      }
    ],
    "insights": [
      "목요일 저녁에 계획 세션이 몰려 있어 20분 휴식 블록을 추가하면 좋아요."
    ]
  }
}
```

---

### 클라이언트 메모

- query key: `['schedule', 'schedule-board-get', view]`
- remote service: `remoteScheduleService.getBoard(view)`
- capture fixture: `src/features/schedule/api/fixtures/schedule-board-week.response.json`

---

### 에러

- `400 INVALID_SCHEDULE_VIEW`
- `401 AUTH_REQUIRED`
- `500 INVALID_SCHEDULE_BOARD_PAYLOAD`
