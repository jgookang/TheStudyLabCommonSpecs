## Spec: POST /api/v1/auth/web/refresh

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-web-refresh-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Web refresh endpoint for rotating the cookie-backed session.

---

### Endpoint

- POST /api/v1/auth/web/refresh

---

### Auth

- Use the web same-site refresh cookie as the transport.
- Clients should send requests with `credentials: include`.

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

- Use explicit refresh-session errors when the cookie is missing, invalid, expired, or revoked.

---

### Client Notes

- Keep this contract aligned with captured fixtures and adapter expectations.
- Update this spec when the real backend payload changes, not just the application code.
