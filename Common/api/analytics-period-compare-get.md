## Spec: GET /api/v1/analytics/compare

**유형**: `API`  
**위치**: `Common/api/analytics-period-compare-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 분석 화면에서 선택한 기간과 과목 기준으로 비교 카드와 비교 인사이트를 조회한다.

---

### 엔드포인트

- `GET /api/v1/analytics/compare?period=week|month&subject=all|math|english|korean|inquiry`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### 응답

```json
{
  "data": {
    "period": "month",
    "subject": "math",
    "summaryTitle": "최근 4주 수학 집중도 비교",
    "cards": [
      {
        "id": "compare-study-hours",
        "label": "학습 시간",
        "currentValue": 25,
        "previousValue": 22,
        "unit": "h",
        "deltaLabel": "+3h 증가",
        "direction": "up"
      }
    ],
    "chartSeries": [
      {
        "id": "study-hours",
        "label": "학습 시간",
        "currentValue": 25,
        "previousValue": 22,
        "unit": "h"
      }
    ],
    "insights": [
      "수학은 최근 4주 기준 복습 효과가 가장 선명하게 드러나는 과목입니다."
    ]
  }
}
```

---

### 클라이언트 메모

- query key: `['analytics', 'analytics-period-compare-get', period, subject]`
- remote service: `remoteAnalyticsService.getCompare(period, subject)`
- period 버튼과 subject select는 URL search params와 query key를 함께 갱신한다.
- `chartSeries`는 상세 비교 차트 영역의 원본 데이터로 사용된다.
- capture fixture: `src/features/analytics/api/fixtures/analytics-period-compare-get.response.json`

---

### 에러

- `400 INVALID_ANALYTICS_COMPARE_FILTER`
- `401 AUTH_REQUIRED`
- `500 INVALID_ANALYTICS_COMPARE_PAYLOAD`
