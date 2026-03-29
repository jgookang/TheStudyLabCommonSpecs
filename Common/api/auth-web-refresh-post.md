## Spec: POST /api/v1/auth/web/refresh

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-web-refresh-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Rotate the web refresh session using same-site HttpOnly cookie transport.

---

### Endpoint

- POST /api/v1/auth/web/refresh

---

### Auth

- Use the web same-site refresh cookie as the transport.
- Clients should send requests with `credentials: include`.
- Origin must be in the configured allowlist.

---

### Request

- Headers:
- `Cookie: studypath_web_refresh_token=<refresh-token>`
- Body: none

---

### Response

- `200 OK`
- Body:

```json
{
  "user": {
    "id": "u1",
    "name": "Smoke Student",
    "grade": "high",
    "subscription": "premium"
  },
  "accessToken": "access.u1.<token>"
}
```
- Rotates and re-sets:
- `studypath_web_refresh_token`
- `studypath_session_id`

---

### Validation Rules

- Missing refresh cookie is treated as an explicit auth failure.
- Stale/revoked/invalid refresh session is treated as explicit auth failure.

---

### Errors

- `401 REFRESH_SESSION_NOT_FOUND`
- `401 REFRESH_SESSION_INVALID`
- `403 ORIGIN_NOT_ALLOWED`

---

### Client Notes

- Web clients must treat refresh `401` responses as signed-out state.
