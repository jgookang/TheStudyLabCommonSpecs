## Spec: timerStore

**타입**: `Store Slice`
**위치**: `Clinet/store/timer-store.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> `timerStore`는 Pomodoro 기반 학습 타이머의 상태 전이를 관리한다.
> 남은 시간, 총 시간, 세션 카운트, 실행 상태를 한 곳에서 다루고 완료 시 세션 저장 흐름과 연결한다.

---

### 요구사항 (Requirements)

- [ ] `idle`, `running`, `paused`, `completed` 상태를 지원한다.
- [ ] 기본 집중 모드 `25분`을 지원한다.
- [ ] `focus`, `short`, `long` 모드를 지원할 수 있어야 한다.
- [ ] 시작, 일시정지, 리셋, 틱, 완료 액션을 제공한다.
- [ ] 세션 완료 횟수를 기록한다.

---

### 인터페이스 정의 (Interface)

```typescript
type TimerStatus = 'idle' | 'running' | 'paused' | 'completed'
type TimerMode = 'focus' | 'short' | 'long'

interface TimerStoreState {
  status: TimerStatus
  mode: TimerMode
  remaining: number
  totalSeconds: number
  sessionCount: number
  currentSubject?: string
}

interface TimerStoreActions {
  start: () => void
  pause: () => void
  reset: (mode?: TimerMode) => void
  tick: () => void
  complete: () => void
  setMode: (mode: TimerMode) => void
  setSubject: (subject?: string) => void
}

type TimerStore = TimerStoreState & TimerStoreActions
```

---

### 상태 전이

| 현재 상태 | 액션 | 다음 상태 |
|----------|------|----------|
| `idle` | `start()` | `running` |
| `running` | `pause()` | `paused` |
| `paused` | `start()` | `running` |
| `running` | `complete()` | `completed` |
| `any` | `reset()` | `idle` |

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `tick()` 호출 | `running` 상태에서만 `remaining`을 감소시킨다 |
| `remaining === 0` | `complete()`를 호출한다 |
| `complete()` | 상태를 완료로 바꾸고 세션 수를 증가시킨다 |
| `reset(mode)` | 해당 모드의 기본 시간으로 초기화한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] `tick()`이 중복 실행되어 시간이 두 번씩 줄지 않도록 해야 한다.
- [ ] 음수 시간으로 내려가지 않도록 clamp가 필요하다.
- [ ] 탭 비활성화나 백그라운드 전환 시 시간 오차 보정 전략이 필요하다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] `start()` 후 상태가 `running`이 된다.
- [ ] `pause()` 후 시간이 유지된다.
- [ ] `tick()`이 남은 시간을 1초 줄인다.
- [ ] 시간이 0이 되면 `completed`가 된다.
- [ ] `reset()`이 기본 시간으로 복귀시킨다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | timerStore Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 상태 전이 확인
- [ ] 모드 범위 확인
