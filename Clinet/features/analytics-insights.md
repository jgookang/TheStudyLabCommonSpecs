## Spec: Analytics Insights

**유형**: `Feature`  
**위치**: `Clinet/features/analytics-insights.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> 학습 시간, 목표 달성률, 과목별 진척도와 기간 비교 흐름을
> `/analytics` 화면에서 일관된 데이터 구조로 제공한다.

---

### 범위

- [x] `/analytics`
- [x] overview metric
- [x] 주간 추이 보드
- [x] 과목별 진척도 보드
- [x] period compare
- [x] subject filter
- [x] 상세 비교 차트
- [ ] drill-down

---

### 현재 구현 기준

- [x] `analytics-overview-get` query hook
- [x] `analytics-period-compare-get` query hook
- [x] mock / remote analytics service
- [x] overview metric, 주간 추이, 과목별 진척도, compare card, insight 카드
- [x] period 버튼과 subject select 기반 compare filter
- [x] URL search param 기반 filter 동기화
- [x] `chartSeries` 기반 상세 비교 차트
- [ ] drill-down 상세 화면

---

### 다음 구현

- [x] `analytics-period-compare-get` spec 추가
- [x] subject / period filter 정책 구체화
- [x] 상세 비교 차트 데이터 계약 정리
- [ ] compare 결과를 dashboard insight와 연결할지 결정
- [ ] 실제 서버 payload 기준 chart tooltip / 범례 정책 정리
