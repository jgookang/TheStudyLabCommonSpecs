## Spec: Backend Phase 1 Plan

**Type**: `Feature`  
**Location**: `Server/features/backend-phase-1-plan.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Lock the implementation order for the first backend slice and track completion status.

---

### Source Specs

- [session-handoff.md](/C:/Users/jgook/repo/CommonSpecs/Server/features/session-handoff.md)
- [phase-1-next-steps.md](/C:/Users/jgook/repo/CommonSpecs/Server/features/phase-1-next-steps.md)
- [auth-session-lifecycle.md](/C:/Users/jgook/repo/CommonSpecs/Server/backend/auth-session-lifecycle.md)
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-logout-post.md`

---

### Locked Decisions

- One backend serves both web and mobile clients.
- Web auth is same-site cookie transport.
- Web refresh failures must return explicit `401`.
- Mobile auth uses request-body refresh token transport on top of the same auth core.

---

### Implementation Sequence (2026-03-29)

1. Step 1: Runtime baseline and auth policy lock - `Done`
2. Step 2: Minimal server bootstrap and router - `Done`
3. Step 3: Shared auth core with token rotation - `Done`
4. Step 4: Web auth endpoints (`/auth/web/*`) - `Done`
5. Step 5: Dashboard P0 read endpoints with auth guard - `Done`
6. Step 6: Curriculum/Schedule/Habit/Analytics/Community read endpoints - `Done`
7. Step 7: Schedule/Habit/Community/Dashboard mutations - `Done`
8. Step 8: Mobile auth endpoints (`/auth/mobile/*`) - `Done`
9. Step 9: Cross-repo smoke/spec sync - `In Progress`

---

### Definition of Done

- Web auth smoke path passes on real backend origin.
- Dashboard P0 reads return stable responses.
- Mobile auth transport contract is implemented and tested.
- Shared specs are synced with actual payloads and errors.

---

### Remaining Work to Close Phase 1

- Finalize Step 9 client-side commit in `TheStudyLab` (smoke script + fixture updates).
- Keep `CommonSpecs` runbook/results synced with the latest smoke output.
- Open Phase 2 scope for persistence, session expiry, and security hardening.
