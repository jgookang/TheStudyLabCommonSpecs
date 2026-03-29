## Spec: POST /api/v1/auth/mobile/login

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-mobile-login-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Start a mobile session and return both access and refresh tokens in the response body.

---

### Endpoint

- POST /api/v1/auth/mobile/login

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
  "accessToken": "access.u1.<token>",
  "refreshToken": "<refresh-token>"
}
```

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

- Mobile clients must store `refreshToken` in secure storage.
