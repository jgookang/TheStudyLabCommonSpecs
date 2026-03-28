## Spec: MetricCard

**타입**: `Component`
**위치**: `Clinet/components/MetricCard.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> MetricCard는 대시보드 상단의 핵심 수치를 요약해서 보여주는 카드다.
> 값, 단위, 추세, 비교 문구를 한정된 공간 안에서 빠르게 읽히도록 구성한다.

---

### 요구사항 (Requirements)

- [ ] label, value, trend 정보를 표시한다.
- [ ] `up`, `down`, `neutral` 추세를 시각적으로 구분한다.
- [ ] 값 옆의 단위를 자연스럽게 조합할 수 있어야 한다.
- [ ] 카드 높이가 데이터 길이에 따라 크게 흔들리지 않아야 한다.
- [ ] 대시보드 기간 탭 전환에 따른 값 갱신 애니메이션을 수용할 수 있어야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
type MetricTrend = 'up' | 'down' | 'neutral'

interface MetricCardProps {
  label: string
  value: string | number
  unit?: string
  trend?: MetricTrend
  trendValue?: string
  trendLabel?: string
  loading?: boolean
  className?: string
}
```

---

### 스타일 기준

- [ ] label은 `text-micro` 또는 `text-caption` 수준으로 표현한다.
- [ ] value는 시선이 가장 먼저 가도록 큰 타이포를 사용한다.
- [ ] `up`은 primary 계열, `down`은 danger 계열을 사용한다.
- [ ] 보조 문구는 secondary 텍스트 톤을 사용한다.

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `loading` | skeleton 형태로 대체한다 |
| trend 존재 | 값 하단에 비교 문구를 노출한다 |
| unit 존재 | value 옆에 축약 단위를 안정적으로 표시한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 값 길이가 길 때도 줄바꿈보다 축약 또는 크기 조정 전략을 우선 검토한다.
- [ ] 단위가 붙는 경우 baseline이 어색하지 않아야 한다.
- [ ] 숫자 애니메이션이 과도하면 가독성이 떨어지지 않도록 duration을 제한해야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 기본 metric이 정상 렌더링된다.
- [ ] trend 상태별 색상/문구가 다르게 표시된다.
- [ ] loading 상태에서 skeleton이 표시된다.
- [ ] unit이 있어도 레이아웃이 무너지지 않는다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | MetricCard 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 추세 표현 방식 확인
- [ ] 값/단위 타이포 비율 확인
