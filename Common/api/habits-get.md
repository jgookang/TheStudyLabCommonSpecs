## Spec: GET /api/v1/dashboard/habits

**유형**: `API Endpoint`  
**위치**: `Common/api/habits-get.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> 대시보드의 `습관 & 성장` 카드에서 사용하는 주간 XP, 레벨, 배지 요약, 루틴 진행 현황을 제공한다.

---

### 요청

- Method: `GET`
- Path: `/api/v1/dashboard/habits`
- Auth: `Bearer access token`

---

### 응답

```json
{
  "weeklyXp": 620,
  "weeklyXpTarget": 1200,
  "level": 5,
  "badgeLabel": "집중 세션 마스터 달성",
  "habits": [
    {
      "id": "habit-reading",
      "title": "아침 30분 영어 읽기",
      "streakDays": 12,
      "completedCount": 5,
      "targetCount": 7,
      "xpReward": 180,
      "color": "#378add"
    },
    {
      "id": "habit-review",
      "title": "오답 노트 복습",
      "streakDays": 8,
      "completedCount": 4,
      "targetCount": 5,
      "xpReward": 240,
      "color": "#ef9f27"
    },
    {
      "id": "habit-focus",
      "title": "집중 세션 3회 이상",
      "streakDays": 14,
      "completedCount": 7,
      "targetCount": 7,
      "xpReward": 440,
      "color": "#1d9e75"
    }
  ]
}
```

---

### Query Key

- React Query key: `['dashboard', 'habits-get', mode]`

---

### 구현 메모

- mock 환경에서는 dashboard summary를 별도 하드코딩하지 않고, habit hub 체크리스트 snapshot에서 파생한다.
- `weeklyXp`, `weeklyXpTarget`, `level`, `badgeLabel`은 habit hub overview / badge 상태와 같은 규칙을 따라야 한다.
- habit checklist mutation이 성공하면 dashboard habits query도 같은 규칙으로 즉시 동기화한다.

---

### 완료 기준

- [x] mock service가 동일한 구조의 데이터를 반환한다.
- [x] dashboard summary가 habit hub 데이터와 같은 규칙에서 파생된다.
- [x] dashboard query key 이름이 spec 문서명과 일치한다.
- [x] remote fixture와 adapter 테스트가 현재 응답 예시를 검증한다.
- [ ] remote service가 실제 endpoint 응답과 연결된다.
