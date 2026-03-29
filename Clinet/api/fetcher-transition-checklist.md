## Spec: Fetcher Transition Checklist

**Type**: `Client API Guide`  
**Location**: `Clinet/api/fetcher-transition-checklist.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Common checklist for moving a feature from mock data to real `/api/v1` fetchers safely.

---

### Transition Sequence

1. Confirm request/response contract in `Common/api`.
2. Confirm endpoint path in `src/shared/api/endpoints.ts`.
3. Update remote service to consume real DTO shape.
4. Keep adapter DTO-to-UI mapping stable.
5. Update query keys and invalidation paths.
6. Update service tests and adapter tests.
7. Enable feature-level `VITE_*_DATA_SOURCE=remote`.
8. Run remote smoke and capture fixtures.
9. Sync fixture/spec/test deltas in one commit.

---

### Common Checklist

- Endpoint path matches latest shared spec.
- Auth transport is correct for web or mobile.
- Nullable and optional fields are handled in adapter tests.
- Empty/error/unauthenticated responses are covered.
- Environment defaults remain explicit and documented.
- Remote smoke passes against real backend origin.

---

### Feature Priority

1. Web auth (`/api/v1/auth/web/*`)
2. Dashboard reads
3. Curriculum/Schedule/Habit/Analytics/Community reads
4. Schedule/Habit/Community mutations

---

### Done Criteria

- Remote mode supports core user flows without mock fallback.
- Smoke command passes with fixture capture enabled.
- Shared specs and client fixtures are aligned with real payloads.
