## Spec: POST /api/v1/auth/web/login

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-web-login-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Start a same-site web session and issue an access token with refresh cookie transport.

---

### Endpoint

- POST /api/v1/auth/web/login

---

### Auth

- This endpoint is public.
- Credential validation still applies.
- Origin must be in the configured allowlist.

---

### Request

- Headers:
- `Content-Type: application/json`
- JSON body:

```json
{
  "email": "student@studypath.dev",
  "password": "demo1234"
}
```

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
- Sets two cookies:
- `studypath_web_refresh_token` (refresh cookie, path `/api/v1/auth/web`)
- `studypath_session_id` (session cookie, path `/api/v1`)
- Cookie flags: `HttpOnly`, `SameSite=Lax`, `Secure` by environment.

---

### Validation Rules

- `email` and `password` are required non-empty strings.
- Invalid JSON body returns `400 INVALID_JSON_PAYLOAD`.

---

### Errors

- `400 INVALID_JSON_PAYLOAD`
- `400 INVALID_CREDENTIALS_PAYLOAD`
- `401 INVALID_CREDENTIALS`
- `403 ORIGIN_NOT_ALLOWED`

---

### Client Notes

- Web clients must use `credentials: include` for follow-up refresh and logout calls.
