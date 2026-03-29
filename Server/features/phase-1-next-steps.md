## Spec: Phase 1 Next Steps

**Type**: `Feature`  
**Location**: `Server/features/phase-1-next-steps.md`  
**Updated**: 2026-03-29  
**Status**: `In Progress`

---

### Purpose

> Track what is left after backend core implementation so cross-repo rollout can close cleanly.

---

### Current State Summary

- Backend implementation steps 1 through 8 are complete in `TheStudyLabServer`.
- Web/mobile split auth endpoints are implemented and tested.
- Dashboard/read and core mutation endpoints are implemented and tested.
- Remote smoke path has passed with `/api/v1/auth/web/*` endpoints.

---

### Immediate Next Steps

1. Finalize Step 9 client repo commit:
- `TheStudyLab/scripts/remoteSmokeTest.mjs`
- refreshed fixture payload files captured from backend
2. Keep `CommonSpecs` runbook and result docs up to date with latest smoke run.
3. Re-run smoke after final client commit to confirm no regression.
4. Move to Phase 2 planning:
- durable storage for sessions/domain data
- session expiry enforcement beyond in-memory runtime
- auth hardening (rate limits, audit logs, revocation tooling)

---

### Parallel Work Items

- Backend Phase 2 design can start in parallel with frontend fixture finalization.
- Contract-test strengthening for shared DTO validation can run in parallel with persistence design.
