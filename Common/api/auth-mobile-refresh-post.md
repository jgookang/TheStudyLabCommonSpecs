## Spec: POST /api/v1/auth/mobile/refresh

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-mobile-refresh-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Mobile refresh endpoint for rotating the stored refresh token.

---

### Endpoint

- POST /api/v1/auth/mobile/refresh

---

### Auth

- Use the mobile refresh token transport.
- Clients should rotate stored refresh tokens after successful refresh.

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

- Use explicit refresh-session errors when the token is missing, invalid, expired, or revoked.

---

### Client Notes

- Keep this contract aligned with captured fixtures and adapter expectations.
- Update this spec when the real backend payload changes, not just the application code.
