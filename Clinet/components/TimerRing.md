## Spec: TimerRing

**타입**: `Component`
**위치**: `Clinet/components/TimerRing.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> TimerRing은 원형 SVG 진행률로 남은 집중 시간을 시각화한다.
> Pomodoro 타이머의 핵심 피드백 요소로서 시간 숫자와 시각 진행 상태를 동시에 제공한다.

---

### 요구사항 (Requirements)

- [ ] 원형 배경 트랙과 진행 아크를 렌더링한다.
- [ ] 남은 시간(`remainingSeconds`)에 따라 진행률을 계산한다.
- [ ] 중앙에 `mm:ss` 형식의 시간 텍스트를 표시한다.
- [ ] 상태 라벨(예: `집중 모드`)을 하단에 표시할 수 있다.
- [ ] `running`, `paused`, `completed` 상태를 반영할 수 있어야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
type TimerStatus = 'idle' | 'running' | 'paused' | 'completed'

interface TimerRingProps {
  totalSeconds: number
  remainingSeconds: number
  status: TimerStatus
  label?: string
  size?: number
  strokeWidth?: number
  color?: string
  backgroundColor?: string
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 초기 렌더 | 전체 시간 기준으로 아크를 계산한다 |
| 매초 갱신 | `stroke-dashoffset`과 텍스트를 업데이트한다 |
| `paused` | 현재 위치를 유지한다 |
| `completed` | 진행률이 끝 상태를 나타내고 완료 스타일을 허용한다 |

---

### 계산 규칙

- [ ] 진행률 비율: `1 - remainingSeconds / totalSeconds`
- [ ] 텍스트 포맷: `mm:ss`
- [ ] `totalSeconds <= 0`이면 0 진행률로 안전하게 처리한다.

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] `remainingSeconds`가 음수가 되지 않도록 방어해야 한다.
- [ ] 매우 작은 컴포넌트 크기에서도 텍스트가 깨지지 않아야 한다.
- [ ] 잦은 재렌더에서도 애니메이션이 과도하게 끊기지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] `25:00` 기본 시간이 올바르게 렌더링된다.
- [ ] 남은 시간이 줄어들면 텍스트와 아크가 함께 업데이트된다.
- [ ] `paused` 상태에서 값이 유지된다.
- [ ] `completed` 상태에서 진행이 끝까지 표시된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | TimerRing 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 표시 형식 확인
- [ ] 상태 표현 범위 확인
