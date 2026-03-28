## Spec: GET /api/v1/analytics/overview

**유형**: `API`  
**위치**: `Common/api/analytics-overview-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 분석 화면에서 overview metric, 주간 추이, 과목별 집중도 데이터를 조회한다.

---

### 엔드포인트

- `GET /api/v1/analytics/overview`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### 응답

```json
{
  "data": {
    "metrics": [
      {
        "id": "weekly-hours",
        "label": "주간 학습 시간",
        "value": 18.5,
        "unit": "h",
        "trendLabel": "지난주 대비 +2.5h"
      }
    ],
    "trend": [
      {
        "id": "mon",
        "label": "월",
        "studyHours": 2.5,
        "goalRate": 72
      }
    ],
    "subjects": [
      {
        "id": "math",
        "subject": "수학",
        "studyHours": 7.2,
        "progressRate": 82
      }
    ],
    "insights": [
      "수학 학습 시간이 가장 크게 늘었고, 목표 달성률도 함께 상승하고 있습니다."
    ]
  }
}
```

---

### 클라이언트 메모

- query key: `['analytics', 'analytics-overview-get']`
- remote service: `remoteAnalyticsService.getOverview()`
- data source env: `VITE_ANALYTICS_DATA_SOURCE`
- capture fixture: `src/features/analytics/api/fixtures/analytics-overview-get.response.json`

---

### 에러

- `401 AUTH_REQUIRED`
- `500 INVALID_ANALYTICS_OVERVIEW_PAYLOAD`
