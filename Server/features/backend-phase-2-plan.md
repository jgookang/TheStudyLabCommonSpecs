## Spec: Backend Phase 2 Plan

**Type**: `Feature`  
**Location**: `Server/features/backend-phase-2-plan.md`  
**Updated**: 2026-03-29  
**Status**: `In Progress`

---

### Purpose

> Define the first implementation slice after Phase 1 closure, focused on durability and security hardening.

---

### Phase 1 Inputs

- Phase 1 implementation steps 1 through 9 are complete.
- Step 9 client finalization commit:
- `bc99fdf chore(smoke): sync web auth paths and refreshed remote fixtures`
- Post-commit remote smoke passed on `http://127.0.0.1:8080`.

---

### Phase 2 Goals

1. Move session/domain state from in-memory runtime assumptions to durable persistence.
2. Enforce session lifecycle guarantees (expiry, revocation, recovery paths).
3. Add baseline auth hardening controls (rate-limit, audit log, operator tooling).
4. Preserve existing API contracts for web/mobile auth and current feature endpoints.

---

### Workstreams

1. Persistence Foundation
- Select storage approach and migration path.
- Introduce repository boundaries for auth/session and mutable feature data.
- Define seed/bootstrap strategy for local and smoke environments.

2. Session Lifecycle Hardening
- Enforce idle + absolute expiry checks on refresh.
- Add revocation lookup/write flow.
- Define logout/logout-all behavior for multi-session states.

3. Auth Security Hardening
- Add per-route rate limits for login/refresh endpoints.
- Capture audit events for auth success/failure and session mutation.
- Define minimal operator/admin revocation workflow.

4. Verification Expansion
- Add persistence-backed integration tests for auth core and transports.
- Add contract tests for `REFRESH_SESSION_*` and expiry/revocation error envelopes.
- Keep remote smoke path green with unchanged endpoint contracts.

---

### Initial Implementation Slice (Recommended)

1. Add pluggable persistence interfaces while keeping current in-memory adapter as fallback.
2. Wire auth core session/token operations through repository interfaces.
3. Add integration tests that run the same auth flow against persistence-backed adapters.
4. Re-run remote smoke and document outcomes in `Clinet/api/remote-smoke-test-results.md`.

---

### Implementation Progress (2026-03-29)

#### Completed in Slice 1

1. Persistence/Repository foundation
- Added pluggable persistence adapter with in-memory fallback.
- Added repository boundaries for auth/session and mutation-heavy features.

2. Session lifecycle hardening
- Added absolute + idle expiry checks.
- Added expiry-triggered revocation path for stale sessions.

3. Auth hardening baseline
- Added login/refresh rate limit controls.
- Added audit event capture for auth success/failure, rate-limit, and logout flows.

4. Verification expansion
- Added persistence-backed integration tests for web/mobile auth across restart.
- Added contract tests for `REFRESH_SESSION_EXPIRED` and `AUTH_RATE_LIMITED`.
- Re-ran remote smoke after implementation (`2026-03-29 10:23 KST`) and confirmed pass.

#### Next Slice

1. Expand operator tooling from single-user scope to bulk/criteria revoke workflows.
2. Add pagination/cursor model and retention policy controls for audit export.
3. Expand persistence-backed coverage for additional mutable feature domains.

---

### Slice 2 Progress (2026-03-29)

#### Completed

1. Operator revocation workflow
- Added operator-protected revoke endpoint:
- `POST /api/v1/auth/operator/sessions/revoke`
- Added operator-protected revoke-all endpoint:
- `POST /api/v1/auth/operator/sessions/revoke-all`
- Added explicit not-found contract for unknown user in revoke-all path.

2. Operator audit query/export
- Added operator-protected audit query endpoint:
- `GET /api/v1/operator/audit/events`
- Added operator-protected audit export endpoint:
- `GET /api/v1/operator/audit/events/export?format=jsonl`
- Added query validation for limit/clientType/timestamp filters.

3. Verification
- Added operator auth + revoke/revoke-all HTTP tests.
- Added operator audit query/export HTTP tests.
- Full server test suite passed (`node --test`, 30 tests).
- Remote smoke passed after slice-2 changes (`2026-03-29 12:14 KST`).

#### Next Slice

1. Add operator pagination/cursor and saved-filter support for audit tooling.
2. Add revoke-all-by-criteria workflows (clientType + time window + active-only guards).
3. Extend persistence-backed coverage to non-auth mutation domains.

---

### Slice 3 Progress (2026-03-29)

#### Completed

1. Operator audit pagination/cursor and filter presets
- Added cursor pagination metadata to operator audit query:
- `GET /api/v1/operator/audit/events`
- Added cursor metadata headers to operator audit export:
- `GET /api/v1/operator/audit/events/export?format=jsonl`
- Added operator-protected saved filter endpoints:
- `GET /api/v1/operator/audit/filters`
- `POST /api/v1/operator/audit/filters`
- `DELETE /api/v1/operator/audit/filters/:filterId`

2. Revoke-all-by-criteria workflow
- Added operator-protected criteria revoke endpoint:
- `POST /api/v1/auth/operator/sessions/revoke-all-by-criteria`
- Added scope guards and payload validation for criteria fields:
- `clientType`, `createdFrom`, `createdTo`, `activeOnly`, `maxRevocations`

3. Persistence-backed verification expansion
- Added persistence restart coverage for mutation domains beyond auth core:
- dashboard plan mutation persistence
- schedule mutation persistence
- habit mutation persistence
- community mutation persistence

4. Verification
- Full server test suite passed (`node --test`, 34 tests).
- Remote smoke was not re-run inside this sandbox session due process spawn policy restriction.
- Last known remote smoke pass remains `2026-03-29 12:14 KST`.

#### Next Slice

1. Re-run remote smoke outside sandbox restrictions and append fresh results in `Clinet/api/remote-smoke-test-results.md`.
2. Introduce audit retention policy controls (event window and export boundaries).
3. Define operator workflow hardening for bulk actions (dry-run/approval/log annotations).

---

### Phase 3 Kickoff Progress (2026-03-29)

#### Completed

1. Audit retention policy controls
- Added `AUTH_AUDIT_RETENTION_DAYS` configuration.
- Added time-based audit event pruning in audit log domain.
- Added retention coverage tests for enabled/disabled modes.

2. Operator bulk-action hardening (dry-run baseline)
- Added `dryRun` support for:
- `POST /api/v1/auth/operator/sessions/revoke-all-by-criteria`
- Added preview response fields for dry-run:
- `candidateCount`, `previewSessionIds`, `reachedLimit`, `dryRun`
- Added validation for dry-run payload typing.

3. Operator approval confirmation workflow
- Added approval-token execution gate for criteria revoke:
- first call: `dryRun=true` to receive `approvalId` and `approvalToken`
- second call: execute with `approvalId` + `approvalToken`
- Added replay/expiry validation contracts:
- `OPERATOR_APPROVAL_INVALID_TOKEN`
- `OPERATOR_APPROVAL_ALREADY_USED`
- `OPERATOR_APPROVAL_EXPIRED`

4. Verification
- Full server test suite passed (`node --test`, 38 tests).
- Inline remote smoke sequence passed (`2026-03-29 13:47 KST`) on `http://127.0.0.1:8080`.
- Full `npm run smoke:remote` orchestration remained blocked in this sandbox session due process policy.

5. Operational runbook sync
- Published environment-tier defaults and operator approval handling guide:
- `Server/backend/operator-security-runbook.md`
- Published deploy-ready server templates in implementation repo:
- `TheStudyLabServer/.env.staging.example`
- `TheStudyLabServer/.env.production.example`

6. Operator evidence capture
- Captured dry-run + execute sample and audit trail summary:
- `Server/backend/operator-security-evidence.md`

7. PostgreSQL migration start
- Added persistence driver mode split: `memory | file | postgres`.
- Added PostgreSQL bootstrap contract for JSONB snapshot table (`studylab_state_snapshots`).
- Added configuration/env contract for PostgreSQL selection and table/schema overrides.
- Added restart validation runner in implementation repo: `npm run smoke:postgres`.
- `pg` install completed; current block is PostgreSQL credential auth failure (`28P01`) on local retry.
- Added relational schema bootstrap runner (`npm run db:bootstrap:postgres`), currently blocked by same credential issue.
- Added dev-only PostgreSQL lifecycle scripts for manual start/stop:
- `db:dev:start`, `db:dev:stop`, `db:dev:status`, `db:dev:url`
- Captured blocked smoke attempt evidence:
- `Server/backend/postgres-smoke-evidence.md`
- Added relational schema bootstrap runner:
- `npm run db:bootstrap:postgres`

#### Next Slice

1. Re-run remote smoke outside sandbox restrictions and record fresh timestamp.
2. Apply template defaults to deployment environment variables and secret manager values.
3. Run staging PostgreSQL connection + smoke verification with `PERSISTENCE_DRIVER=postgres` and `smoke:postgres`.
4. Run relational schema bootstrap on staging PostgreSQL and capture output evidence.

---

### Out of Scope for First Slice

- Full analytics/event warehouse ingestion.
- Advanced anomaly detection and risk scoring.
- Multi-region replication.
