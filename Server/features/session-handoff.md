## Spec: Session Handoff

**Type**: `Handoff`  
**Location**: `Server/features/session-handoff.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Server-side handoff summary for the next engineer.

---

### Core Focus

- Keep backend work aligned with the shared contracts under `Common/api`.
- Prefer a narrow, stable first slice over broad partial coverage.
- Preserve enough context that another engineer can continue without rediscovery.

---

### Operational Notes

- Record auth, payload, and rollout decisions in the relevant shared or backend spec as soon as they become stable.
- Use smoke-driven verification to confirm the first real backend path.
