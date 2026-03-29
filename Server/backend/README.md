# Backend Design Notes
Backend design docs describe how the server should satisfy the shared contracts in Common/ while keeping web and mobile transport differences explicit.
### Primary Topics
- Auth and session lifecycle
- Session storage and rotation strategy
- Middleware and authorization context
- Operational guardrails for logging, rate limits, and revocation
- Operator security runbook defaults and bulk-revoke execution policy
- Operator security evidence capture and incident appendix sample
### How To Use This Folder
- Review the shared endpoint contracts first.
- Lock transport decisions here before implementation starts in the server repository.
- Use these notes to drive smoke-test readiness and endpoint rollout order.
- Keep `operator-security-runbook.md` aligned with real runtime defaults.
- Keep `operator-security-evidence.md` refreshed with the latest validation sample.
