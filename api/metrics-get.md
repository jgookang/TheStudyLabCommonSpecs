## Spec: metrics-get

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/metrics-get.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 대시보드 상단 지표 카드에 필요한 요약 수치를 조회한다.  
> 기간(`daily`, `weekly`, `monthly`)에 따라 학습 시간, 목표 달성률, 습관 점수, 연속 학습일을 반환한다.

---

### 요구사항

- [x] `period` query parameter 를 지원한다.
- [x] 4개의 핵심 지표를 반환한다.
- [x] 각 지표는 value, unit, trend 정보를 포함한다.
- [x] 인증이 필요하다.

---

### 인터페이스 정의

```typescript
type DashboardPeriod = 'daily' | 'weekly' | 'monthly'
type MetricTrend = 'up' | 'down' | 'neutral'

interface MetricsGetRequest {
  period: DashboardPeriod
}

interface MetricItem {
  id: 'study-time' | 'goal-rate' | 'habit-score' | 'streak-days'
  label: string
  value: string | number
  unit?: string
  trend: MetricTrend
  trendValue: string
  trendLabel: string
}

interface MetricsGetResponse {
  data: MetricItem[]
}

interface ApiErrorResponse {
  code: string
  message: string
  details?: unknown
}
```

---

### 서버 검증 규칙

| 필드 | 규칙 |
|------|------|
| `period` | 필수. `daily`, `weekly`, `monthly` 중 하나여야 한다. |
| Authorization | 필수. 유효한 access token 또는 서버 인증 컨텍스트가 있어야 한다. |

검증 실패 시 권장 에러:
- 잘못된 `period`: `400 INVALID_PERIOD`
- 인증 없음: `401 AUTH_REQUIRED`

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| `period=daily` | 오늘 기준 지표를 반환한다. |
| `period=weekly` | 이번 주 기준 누적/비교 지표를 반환한다. |
| `period=monthly` | 이번 달 기준 지표를 반환한다. |

---

### Query Key / Adapter

- React Query key: `['dashboard', 'metrics-get', period, mode]`
- remote DTO 는 `data: MetricItem[]` 구조를 사용하고 화면에서는 그대로 metric 모델로 사용한다.

---

### 엣지 케이스

- [x] 지표 데이터가 비어 있으면 빈 배열을 반환할 수 있다.
- [x] 지원하지 않는 `period` 값은 validation error 를 반환해야 한다.
- [x] 인증이 없으면 401 계열 에러를 반환한다.

---

### 테스트 시나리오

- [x] daily 요청은 정상 응답한다.
- [x] weekly 요청은 정상 응답한다.
- [ ] 잘못된 `period` 요청은 `400 INVALID_PERIOD` 를 반환한다.
- [ ] 인증 없는 요청은 `401 AUTH_REQUIRED` 를 반환한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | metrics 조회 API spec 초안 작성 | Codex |
| 2026-03-28 | React Query key 와 remote DTO 구조 기준 반영 | Codex |
| 2026-03-28 | 서버 검증 규칙과 에러 코드 기준 추가 | Codex |
