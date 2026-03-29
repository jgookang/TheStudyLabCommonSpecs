## Spec: GET /api/v1/community/posts/:postId

**Type**: `API Endpoint`  
**Location**: `Common/api/community-post-detail-get.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Community post detail payload, including interaction-ready fields.

---

### Endpoint

- GET /api/v1/community/posts/:postId

---

### Auth

- Require a valid authenticated user context.
- Return `401 AUTH_REQUIRED` when the caller is not signed in.

---

### Request

- Use the route, query, and JSON body defined for $Endpoint.
- Keep required identifiers, enums, and field names stable across client and server changes.

---

### Response

- Return a stable DTO aligned with the client adapter for this screen or mutation.
- Prefer cache-friendly responses so the client can update state without guessing.

---

### Validation Rules

- Validate required fields, route parameters, and supported enum values.
- Keep timezone, ownership, and ordering rules consistent with the surrounding product flow.

---

### Errors

- Use endpoint-specific validation errors together with `401 AUTH_REQUIRED` when authentication is missing.

---

### Client Notes

- Keep this contract aligned with captured fixtures and adapter expectations.
- Update this spec when the real backend payload changes, not just the application code.
