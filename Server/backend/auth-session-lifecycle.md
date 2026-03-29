## Spec: Auth Session Lifecycle

**Type**: `Backend Design`  
**Location**: `Server/backend/auth-session-lifecycle.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Define one shared auth core that serves both the web frontend and the mobile app, with transport rules separated by client type.

---

### Source Specs

- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-logout-post.md`

---

### Core Policy

- Keep a single auth core for session creation, refresh rotation, and session revoke.
- Split only the refresh transport:
- Web: same-site HttpOnly cookie transport.
- Mobile: request-body refresh token transport.
- Web refresh failures must always return explicit `401` codes so the client can transition to signed-out state safely.

---

### Shared Session Core

- `accessToken` is issued on both login and refresh.
- Refresh tokens are rotated on every successful refresh.
- Refresh tokens are stored as hash values, not raw token strings.
- Session records are keyed by `clientType = 'web' | 'mobile'`.

```typescript
interface AuthSessionRecord {
  id: string
  userId: string
  clientType: 'web' | 'mobile'
  refreshTokenHash: string
  createdAt: string
  updatedAt: string
  lastUsedAt: string | null
  revokedAt: string | null
}
```

---

### Web Transport Contract

#### Endpoints

- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `POST /api/v1/auth/web/logout`

#### Cookie Policy

- Refresh cookie name: `studypath_web_refresh_token` (configurable).
- Session cookie name: `studypath_session_id` (configurable).
- Refresh cookie path: `/api/v1/auth/web`.
- Session cookie path: `/api/v1`.
- Cookie flags: `HttpOnly`, `SameSite=Lax`, `Secure` via environment config.
- Default cookie max-age:
- Refresh cookie: `604800` seconds (7 days).
- Session cookie: `86400` seconds (1 day).

#### Refresh Failure Policy

- Missing refresh cookie: `401 REFRESH_SESSION_NOT_FOUND`
- Invalid or revoked session: `401 REFRESH_SESSION_INVALID`

---

### Mobile Transport Contract

#### Endpoints

- `POST /api/v1/auth/mobile/login`
- `POST /api/v1/auth/mobile/refresh`
- `POST /api/v1/auth/mobile/logout`

#### Policy

- Mobile clients store refresh tokens in secure app storage.
- Mobile refresh uses request body: `{ "refreshToken": "..." }`.
- Refresh success returns both a new `accessToken` and a rotated `refreshToken`.
- Refresh failures return explicit `401` codes (`REFRESH_SESSION_NOT_FOUND` or `REFRESH_SESSION_INVALID`).

---

### Shared Protected Endpoint Policy

- Protected endpoints require authenticated context.
- Missing or invalid auth context must return `401 AUTH_REQUIRED`.
- Ownership and role checks are handled after authentication.

---

### Verification Baseline

- Unit:
- Credential validation
- Session create/revoke
- Refresh token rotation
- Web integration:
- Login sets refresh/session cookies
- Refresh rotates cookie and returns new `accessToken`
- Refresh missing-cookie path returns explicit `401`
- Mobile integration:
- Login returns `accessToken + refreshToken`
- Refresh rotates `refreshToken`
- Refresh invalid-token path returns explicit `401`
