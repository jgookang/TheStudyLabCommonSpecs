## Spec: Operator Security Runbook

**Type**: `Backend Design`  
**Location**: `Server/backend/operator-security-runbook.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Define operating defaults and execution policy for audit retention and criteria-based session revocation.

---

### Environment Defaults

- Local development:
- `AUTH_AUDIT_RETENTION_DAYS=30`
- `AUTH_AUDIT_MAX_EVENTS=500`
- `OPERATOR_CRITERIA_APPROVAL_TTL_SECONDS=180`
- `OPERATOR_CRITERIA_APPROVAL_MAX_PENDING=100`
- Staging:
- `AUTH_AUDIT_RETENTION_DAYS=30`
- `AUTH_AUDIT_MAX_EVENTS=5000`
- `OPERATOR_CRITERIA_APPROVAL_TTL_SECONDS=180`
- `OPERATOR_CRITERIA_APPROVAL_MAX_PENDING=200`
- Production:
- `AUTH_AUDIT_RETENTION_DAYS=90`
- `AUTH_AUDIT_MAX_EVENTS=20000`
- `OPERATOR_CRITERIA_APPROVAL_TTL_SECONDS=120`
- `OPERATOR_CRITERIA_APPROVAL_MAX_PENDING=500`

Policy notes:

- `AUTH_AUDIT_RETENTION_DAYS=0` disables time pruning and is not allowed for production defaults.
- Keep approval TTL short to reduce replay and delayed-execution risk.

---

### Criteria Revoke Execution Policy

1. Preview required
- Call `POST /api/v1/auth/operator/sessions/revoke-all-by-criteria` with `dryRun=true`.
- Confirm candidate scope before any execution.
2. Approval-token execute
- Execute only with `approvalId` and `approvalToken` from preview response.
- Do not mutate criteria during execute request.
3. Post-action verification
- Confirm `revokedCount` and affected `sessionIds`.
- Verify audit event trail:
- `GET /api/v1/operator/audit/events?event=auth.operator.revoke_by_criteria&limit=20`

---

### Failure Code Troubleshooting

- `OPERATOR_APPROVAL_NOT_FOUND`:
- approval id missing or pruned; run preview again.
- `OPERATOR_APPROVAL_EXPIRED`:
- approval TTL exceeded; rerun preview and execute promptly.
- `OPERATOR_APPROVAL_ALREADY_USED`:
- replay request blocked by design; do not retry same approval.
- `OPERATOR_APPROVAL_INVALID_TOKEN`:
- token mismatch; rerun preview and execute with new pair.

---

### Operational Checklist

- Record incident reason and operator identity before bulk revoke.
- Prefer narrow criteria over broad windows.
- Set `maxRevocations` for high-risk scopes.
- Export and attach audit evidence after execution.

---

### Implementation Mapping

- Server repo templates:
- `TheStudyLabServer/.env.staging.example`
- `TheStudyLabServer/.env.production.example`
- Server repo evidence:
- `TheStudyLabServer/docs/operator-security-evidence.md`
