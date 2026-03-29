## Spec: Backend Payload Capture

**Type**: `Client API Guide`  
**Location**: `Clinet/api/backend-payload-capture.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Define where real backend payloads are captured and how to sync them into fixtures, tests, and shared specs.

---

### Target Fixture Files

#### Auth

- `src/features/auth/api/fixtures/auth-session.json`
- `src/features/auth/api/fixtures/auth-refresh-session.json`

#### Dashboard

- `src/features/dashboard/api/fixtures/metrics-get.response.json`
- `src/features/dashboard/api/fixtures/notifications-get.response.json`
- `src/features/dashboard/api/fixtures/plans-today-get.response.json`
- `src/features/dashboard/api/fixtures/habits-get.response.json`

#### Schedule

- `src/features/schedule/api/fixtures/schedule-board-week.response.json`
- `src/features/schedule/api/fixtures/schedule-create.response.json`
- `src/features/schedule/api/fixtures/schedule-update.response.json`
- `src/features/schedule/api/fixtures/schedule-delete.response.json`

#### Habit

- `src/features/habit/api/fixtures/habit-hub-get.response.json`
- `src/features/habit/api/fixtures/habit-check.response.json`

---

### Capture Procedure

1. Run remote smoke against real backend with `--capture-fixtures`.
2. Verify each overwritten fixture has valid JSON and expected shape.
3. Update adapter/service tests for any real payload differences.
4. Update matching `Common/api` specs in the same change.
5. Re-run client tests and build checks.

---

### Checklist

- Fixture payload matches real backend response.
- Nullable/optional fields are documented in spec and adapter tests.
- Auth and validation error payloads are covered in tests.
- `npm.cmd test -- --run` passes.
- `npm.cmd run build` passes.
