## Spec: POST /api/v1/auth/web/logout

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-web-logout-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Revoke the current web refresh session and clear auth cookies.

---

### Endpoint

- POST /api/v1/auth/web/logout

---

### Auth

- Use the web same-site refresh cookie as the transport.
- Clients should send requests with `credentials: include`.
- Origin must be in the configured allowlist.

---

### Request

- Headers:
- `Cookie: studypath_web_refresh_token=<refresh-token>` (optional for idempotency)
- Body: none

---

### Response

- `200 OK`
- Body:

```json
{
  "ok": true
}
```
- Always clears:
- `studypath_web_refresh_token`
- `studypath_session_id`

---

### Validation Rules

- Logout is idempotent.
- If no refresh cookie is present, respond success and still clear cookies.

---

### Errors

- `403 ORIGIN_NOT_ALLOWED`

---

### Client Notes

- After logout success, clear local access token state immediately.
