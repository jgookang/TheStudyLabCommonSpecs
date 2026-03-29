## Spec: POST /api/v1/auth/mobile/refresh

**Type**: `API Endpoint`  
**Location**: `Common/api/auth-mobile-refresh-post.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Rotate a mobile refresh token and return the next session token pair.

---

### Endpoint

- POST /api/v1/auth/mobile/refresh

---

### Auth

- Use the mobile refresh token transport.
- Clients should rotate stored refresh tokens after successful refresh.
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
  "refreshToken": "<next-refresh-token>"
}
```

---

### Validation Rules

- `refreshToken` is required and must be a non-empty string.
- Invalid JSON body returns `400 INVALID_JSON_PAYLOAD`.

---

### Errors

- `400 INVALID_JSON_PAYLOAD`
- `401 REFRESH_SESSION_NOT_FOUND`
- `401 REFRESH_SESSION_INVALID`
- `403 ORIGIN_NOT_ALLOWED`

---

### Client Notes

- Always overwrite stored refresh token with the refreshed token from response.
