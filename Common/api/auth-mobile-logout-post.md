## Spec: POST /api/v1/auth/mobile/logout

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-mobile-logout-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Revoke a mobile session by refresh token and return idempotent success.

---

### Endpoint

- POST /api/v1/auth/mobile/logout

---

### Auth

- Use the mobile refresh token transport.
- Origin must be in the configured allowlist.

---

### Request

- Headers:
- `Content-Type: application/json`
- JSON body:

```json
{
  "refreshToken": "<refresh-token>"
}
```
- `refreshToken` is optional for idempotent no-session logout calls.

---

### Response

- `200 OK`
- Body:

```json
{
  "ok": true
}
```

---

### Validation Rules

- Invalid JSON body returns `400 INVALID_JSON_PAYLOAD`.
- Logout is idempotent even when token is missing or already revoked.

---

### Errors

- `400 INVALID_JSON_PAYLOAD`
- `403 ORIGIN_NOT_ALLOWED`

---

### Client Notes

- On success, clear local access/refresh tokens immediately.
