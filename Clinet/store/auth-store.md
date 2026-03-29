## Spec: authStore

**Type**: `Client Store Contract`  
**Location**: `Clinet/store/auth-store.md`  
**Updated**: 2026-03-29  
**Status**: `Ready`

---

### Purpose

> Manage authentication bootstrap, refresh, and signed-in user state for the client app.

---

### State Model

- Track `user`, `accessToken`, `isAuthenticated`, and `isRefreshing` in one place.
- Track bootstrap progress with `initial`, `loading`, `ready`, `timed_out`, and `error` states.
- Use `bootstrapRequestKey` to force retried bootstrap flows when needed.

---

### Actions

- `setSession(payload)` sets the user and access token after login or refresh.
- `clearSession()` removes all session information while leaving bootstrap as complete.
- `startBootstrap()`, `finishBootstrap()`, `markBootstrapTimedOut()`, and `failBootstrap()` manage startup recovery.
- `retryBootstrap()` restarts the recovery flow.
- `startRefresh()` and `finishRefresh()` model refresh in progress and refresh completion.

---

### Operational Rules

- Treat `finishRefresh(null)` as a valid signed-out outcome rather than a crash path.
- The web client uses `/api/v1/auth/web/*` endpoints and treats refresh `401` values as a signed-out state.
- Route guards should wait for bootstrap to finish before redirecting protected screens.
- Timeout and error states should expose a retry CTA instead of causing silent failure.

---

### Test Notes

- Default state is signed out with bootstrap in `initial`.
- Session setters mark the store as authenticated and ready.
- Retry increments the request key so bootstrap effects can rerun.
- Refresh completion with `null` clears the session cleanly.
