## Spec: timerStore

**Type**: `Client Store Contract`  
**Location**: `Clinet/store/timer-store.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Model the Pomodoro-style study timer, including status changes, elapsed progress, and completed sessions.

---

### State Model

- Track `status`, `mode`, `remaining`, `totalSeconds`, `sessionCount`, and the current subject.
- Support `focus`, `short`, and `long` timer modes.
- Support `idle`, `running`, `paused`, and `completed` status values.

---

### Actions

- `start()` moves idle or paused timers into a running state.
- `pause()` freezes the timer without resetting remaining time.
- `reset(mode?)` restores the default duration for the chosen mode.
- `tick()` decrements remaining time while the timer is running.
- `complete()` marks the timer as done and increments the completed session count.
- `setMode(mode)` and `setSubject(subject)` update the timer context.

---

### Operational Rules

- Prevent duplicate ticking so time does not drop twice per second.
- Clamp remaining time at zero and complete automatically when it runs out.
- Background or tab-suspension drift should be corrected by a higher-level effect or service.

---

### Test Notes

- Starting changes the status to `running`.
- Pausing preserves the remaining time.
- Ticking decrements time only while running.
- Reset restores the expected default duration for the active mode.
