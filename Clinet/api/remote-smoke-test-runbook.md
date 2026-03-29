## Spec: Remote Smoke Test Runbook

**Type**: `Client API Guide`  
**Location**: `Clinet/api/remote-smoke-test-runbook.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Verify the first end-to-end remote path (`web auth -> dashboard -> habit -> schedule -> logout`) against a real backend origin.

---

### Script

- `C:\Users\jgook\repo\TheStudyLab\scripts\remoteSmokeTest.mjs`

---

### Command

```powershell
npm.cmd run smoke:remote -- --base-url http://127.0.0.1:8080 --capture-fixtures
```

---

### Required Environment

```env
VITE_API_BASE_URL=http://127.0.0.1:8080
STUDYPATH_SMOKE_EMAIL=student@studypath.dev
STUDYPATH_SMOKE_PASSWORD=demo1234
```

---

### Preconditions

- Backend is running and serves `/api/v1/*`.
- Auth policy is same-site for web refresh cookie transport.
- Frontend and backend origins are allowlisted for CORS/origin checks.

---

### Covered Flow

1. `POST /api/v1/auth/web/login`
2. `POST /api/v1/auth/web/refresh`
3. `GET /api/v1/dashboard/metrics?period=weekly`
4. `GET /api/v1/dashboard/notifications`
5. `GET /api/v1/dashboard/plans/today`
6. `GET /api/v1/dashboard/habits`
7. `GET /api/v1/habits`
8. `POST /api/v1/habits/check`
9. `GET /api/v1/schedule/board?view=week`
10. `GET /api/v1/schedule/board?view=month`
11. `POST /api/v1/schedule`
12. `PATCH /api/v1/schedule/:scheduleId`
13. `DELETE /api/v1/schedule/:scheduleId`
14. `POST /api/v1/auth/web/logout`

---

### Fixture Capture

- Use `--capture-fixtures` to overwrite fixture payloads with real backend responses.
- Keep fixture updates and spec updates in the same change.
- Record every run outcome in `remote-smoke-test-results.md`.

---

### Failure Handling

- If login returns `404`, verify the base URL is a backend origin (not the frontend dev server).
- If refresh returns `401 REFRESH_SESSION_*`, treat it as signed-out and inspect cookie/origin setup.
- If DTO mismatches appear, update adapters and shared specs together.
