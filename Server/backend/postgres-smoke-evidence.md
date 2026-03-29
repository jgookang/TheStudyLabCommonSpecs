## Spec: PostgreSQL Smoke Evidence

**Type**: `Ops Note`  
**Location**: `Server/backend/postgres-smoke-evidence.md`  
**Updated**: 2026-03-29  
**Status**: `In Progress`

---

### Purpose

> Track restart-smoke execution status for PostgreSQL persistence mode.

---

### Attempt 1 (Dependency Blocked)

- Captured at (UTC): `2026-03-29T06:35:21.378Z`
- Captured at (KST): `2026-03-29 15:35:21`
- Command:
- `npm.cmd run smoke:postgres`
- Result:
- failed due missing `pg` dependency at that time.

### Attempt 2 (Dependency Installed, Auth Blocked)

- Captured at (UTC): `2026-03-29T07:27:42.940Z`
- Captured at (KST): `2026-03-29 16:27:42`
- Command:
- `npm.cmd run smoke:postgres -- --database-url "postgresql://postgres:postgres@localhost:5432/postgres"`
- Result:
- failed with PostgreSQL authentication error `28P01`.
- Blocking reason:
- provided local credential is invalid for target PostgreSQL instance.

### Attempt 3 (Relational Bootstrap Script Check)

- Captured at (UTC): `2026-03-29T07:46:02Z`
- Captured at (KST): `2026-03-29 16:46:02`
- Command:
- `npm.cmd run db:bootstrap:postgres -- --database-url "postgresql://postgres:postgres@localhost:5432/postgres"`
- Result:
- failed with PostgreSQL authentication error `28P01`.
- Blocking reason:
- same local credential mismatch against target PostgreSQL instance.

---

### Next Successful Run Criteria

- `npm.cmd install` completed (done in current workspace).
- `npm.cmd run smoke:postgres` returns JSON `"status": "passed"`.
- `npm.cmd run db:bootstrap:postgres` succeeds with `ok: true`.
- Restart validation confirms:
- first boot login/mutation success
- second boot refresh success
- persisted dashboard plan status check success

---

### Source of Truth

- Implementation repo evidence:
- `TheStudyLabServer/docs/postgres-smoke-evidence.md`
