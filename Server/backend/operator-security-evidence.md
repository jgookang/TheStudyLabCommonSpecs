## Spec: Operator Security Evidence

**Type**: `Ops Note`  
**Location**: `Server/backend/operator-security-evidence.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Record a concrete dry-run and execute sample for operator criteria revoke workflow.

---

### Captured Run

- Captured at (UTC): `2026-03-29T05:36:52.588Z`
- Captured at (KST): `2026-03-29 14:36:52`
- Base URL: `http://127.0.0.1:18084`
- Target path:
- `POST /api/v1/auth/operator/sessions/revoke-all-by-criteria`

---

### Result Summary

- Preview (dry-run) status: `200`
- `candidateCount`: `2`
- Execute status: `200`
- `revokedCount`: `2`
- Audit query status: `200`
- Audit event count for `auth.operator.revoke_by_criteria`: `2`
- Event order verified:
- `preview` then `success`

---

### Source Evidence

- Implementation repo evidence snapshot:
- `TheStudyLabServer/docs/operator-security-evidence.md`
