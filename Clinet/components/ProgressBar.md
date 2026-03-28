## Spec: ProgressBar

**타입**: `Component`
**위치**: `Clinet/components/ProgressBar.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> ProgressBar는 진도, 달성률, 경험치 등 비율형 데이터를 시각화하는 공통 컴포넌트다.
> 색상과 크기를 조절할 수 있고, 대시보드와 습관/게이미피케이션 영역에서 재사용된다.

---

### 요구사항 (Requirements)

- [ ] 현재값과 최대값을 기반으로 채움 비율을 계산한다.
- [ ] 색상 커스터마이징을 지원한다.
- [ ] `sm`, `md`, `lg` 크기 또는 높이 프리셋을 지원한다.
- [ ] 선택적으로 진입 애니메이션을 지원한다.
- [ ] 스크린 리더를 위한 진행률 정보가 제공되어야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
type ProgressBarSize = 'sm' | 'md' | 'lg'

interface ProgressBarProps {
  value: number
  max?: number
  color?: string
  size?: ProgressBarSize
  animated?: boolean
  ariaLabel?: string
  className?: string
}
```

---

### 계산 규칙

- [ ] 진행률: `Math.max(0, Math.min(100, (value / max) * 100))`
- [ ] `max` 기본값은 `100`
- [ ] `max <= 0`이면 0%로 안전하게 처리한다.

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `animated=true` | 마운트 시 0에서 목표치까지 부드럽게 전환한다 |
| `value > max` | 최대 100%로 clamp 한다 |
| `value < 0` | 최소 0%로 clamp 한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 극단적인 작은 값도 0처럼 보이지 않도록 최소 가시 폭 정책이 필요할 수 있다.
- [ ] 다양한 배경 위에서도 채움과 트랙의 구분이 유지되어야 한다.
- [ ] 과도한 애니메이션은 연속 리스트에서 시각 피로를 줄 수 있으므로 stagger 기준이 필요하다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 기본 진행률 계산이 정확하다.
- [ ] value가 max를 넘어도 100%를 넘지 않는다.
- [ ] animated 옵션이 있을 때 전환 스타일이 적용된다.
- [ ] aria 속성이 제공된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | ProgressBar 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] size 프리셋 확인
- [ ] 애니메이션 적용 범위 확인
