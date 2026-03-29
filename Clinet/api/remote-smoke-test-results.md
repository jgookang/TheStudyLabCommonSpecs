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

#### 2026-03-29 09:58 KST (Passed, Fixture Capture Refresh)

- Command:

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080 --capture-fixtures
```

- Result: passed.
- Notes:
- Re-captured fixture payloads from current backend runtime.
- Confirmed web auth transport path (`/api/v1/auth/web/*`) and weekly metrics query.

#### 2026-03-29 10:00 KST (Passed, Post-Commit Regression Check)

- Command:

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080
```

- Result: passed.
- Related client commit in `TheStudyLab`:
- `bc99fdf chore(smoke): sync web auth paths and refreshed remote fixtures`
- Verification:
- Same end-to-end path passed after commit with no additional fixture capture.

#### 2026-03-29 10:23 KST (Passed, Phase 2 Slice-1 Regression Check)

- Command:

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080
```

- Result: passed.
- Notes:
- Verified regression after Phase 2 slice-1 server changes:
- persistence adapter + repository boundary introduction
- session lifecycle hardening (idle/absolute expiry + revoke)
- auth hardening baseline (rate-limit + audit event capture)
- No contract break detected on existing smoke path.

#### 2026-03-29 12:14 KST (Passed, Phase 2 Slice-2 Regression Check)

- Command:

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080
```

- Result: passed.
- Notes:
- Verified regression after Phase 2 slice-2 server changes:
- operator session revoke/revoke-all workflow endpoints
- operator audit query/export endpoints
- operator auth guard + audit query validation paths
- No contract break detected on existing smoke path.

#### 2026-03-29 13:47 KST (Passed, Phase 3 Approval-Workflow Regression Check)

- Command:

```powershell
node -e "<inline createApp + fetch smoke runner>"
```

- Result: passed.
- Notes:
- Sandbox process policy blocked background server orchestration (`npm run smoke:remote` path).
- Executed equivalent inline smoke sequence in a single Node process:
- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `GET /api/v1/schedule/board?view=week`
- `POST /api/v1/schedule`
- `GET /api/v1/dashboard/metrics?period=weekly`
- `POST /api/v1/auth/web/logout`
- Verified no contract break after operator criteria approval-token gate changes.

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

- Continue periodic smoke runs after backend auth/session changes.
- Track backend Phase 2 kickoff in `Server/features/backend-phase-2-plan.md`.
