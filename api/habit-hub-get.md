## Spec: GET /api/v1/habits

**유형**: `API`  
**위치**: `docs/specs/api/habit-hub-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> habit 화면에서 streak, XP, 배지 진행률, 루틴 체크리스트를 조회한다.

---

### 엔드포인트

- `GET /api/v1/habits`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### 응답

```json
{
  "data": {
    "overview": {
      "currentStreak": 14,
      "bestStreak": 28,
      "xp": 620,
      "xpTarget": 1200,
      "level": 5
    },
    "checklist": [
      {
        "id": "habit-reading",
        "title": "아침 30분 영어 읽기",
        "cadenceLabel": "매일",
        "streakDays": 12,
        "xpReward": 180,
        "completedToday": true
      }
    ],
    "badges": [
      {
        "id": "badge-focus",
        "title": "집중 세션 마스터",
        "targetLabel": "14일 연속",
        "progressRate": 100,
        "unlocked": true
      }
    ],
    "insights": [
      "현재 가장 긴 루틴은 14일 연속입니다."
    ]
  }
}
```

---

### 클라이언트 메모

- query key: `['habit', 'habit-hub-get']`
- remote service: `remoteHabitService.getHub()`
- data source env: `VITE_HABIT_DATA_SOURCE`
- capture fixture: `src/features/habit/api/fixtures/habit-hub-get.response.json`

---

### 에러

- `401 AUTH_REQUIRED`
- `500 INVALID_HABIT_HUB_PAYLOAD`
