## Spec: Remote Smoke Test Results

**Type**: `Client API Guide`  
**Location**: `Clinet/api/remote-smoke-test-results.md`  
**Updated**: 2026-03-29  
**Status**: `In Progress`

---

### Purpose

> Living log of remote smoke runs, failures, and fixture/spec sync actions.

---

### Execution History

#### 2026-03-28 20:51 KST (Failed)

- Command: `node .\scripts\remoteSmokeTest.mjs --base-url http://localhost:4000`
- Result: failed at login.
- Failure detail: `POST /api/v1/auth/login` returned `404`.
- Root cause: port `4000` was frontend dev server origin, not backend `/api/v1` origin.

#### 2026-03-29 (Passed)

- Command:

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080 --capture-fixtures
```

- Result: passed.
- Auth path updated to web transport endpoints:
- `/api/v1/auth/web/login`
- `/api/v1/auth/web/refresh`
- `/api/v1/auth/web/logout`
- Metrics query updated to `period=weekly`.

---

### Fixture Capture Applied

- `src/features/auth/api/fixtures/auth-session.json`
- `src/features/auth/api/fixtures/auth-refresh-session.json`
- `src/features/dashboard/api/fixtures/metrics-get.response.json`
- `src/features/dashboard/api/fixtures/notifications-get.response.json`
- `src/features/dashboard/api/fixtures/plans-today-get.response.json`
- `src/features/dashboard/api/fixtures/habits-get.response.json`
- `src/features/habit/api/fixtures/habit-hub-get.response.json`
- `src/features/habit/api/fixtures/habit-check.response.json`
- `src/features/schedule/api/fixtures/schedule-board-week.response.json`
- `src/features/schedule/api/fixtures/schedule-create.response.json`
- `src/features/schedule/api/fixtures/schedule-update.response.json`
- `src/features/schedule/api/fixtures/schedule-delete.response.json`

---

### Follow-up Items

- Keep `Common/api` endpoint docs synced with any payload or error-code changes.
- Re-run smoke after backend auth/session changes.
- Keep this file append-only for run history.

---

### Next Check

- Re-run full smoke after Step 9 client commits are finalized in `TheStudyLab`.
