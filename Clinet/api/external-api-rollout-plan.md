## Spec: External API Rollout Plan

**Type**: `Client API Guide`  
**Location**: `Clinet/api/external-api-rollout-plan.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Move the frontend from mock mode to real backend mode in a staged, low-risk sequence.

---

### Rollout Principles

- Keep UI behavior stable while changing only data source and contracts.
- Do not flip all features to remote at once.
- Finish web auth verification before broad read/mutation rollout.
- Sync adapter and fixture updates with shared spec updates.
- Use smoke-first verification for each rollout stage.

---

### Inputs Required from Backend

- Backend origin that serves `/api/v1/*`
- Test account credentials
- Confirmed web auth policy:
- same-site cookie transport
- refresh failures return explicit `401`
- CORS/origin allowlist policy
- Stable error envelope examples (`401`, `403`, `422`, `500`)

---

### Phased Rollout

#### Phase A: Web Auth

- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `POST /api/v1/auth/web/logout`
- Verify bootstrap timeout/error/retry UX and signed-out fallback behavior.

#### Phase B: Read Endpoints

- Dashboard/Curriculum/Schedule/Habit/Analytics/Community reads.
- Validate DTO differences through adapters before touching UI rendering.

#### Phase C: Mutations

- Schedule create/update/delete
- Habit check
- Community comment/like/report
- Validate optimistic update and rollback behavior.

#### Phase D: Sync and Hardening

- Capture fixtures from real responses.
- Update shared specs with request/response and error deltas.
- Re-run smoke and regression checks.

---

### Standard Smoke Command

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080 --capture-fixtures
```

---

### Success Criteria

- Auth/read/mutation core flows pass in remote mode.
- Fixture/spec/test contract is synchronized.
- Diagnostics show target features running in `remote` mode.
