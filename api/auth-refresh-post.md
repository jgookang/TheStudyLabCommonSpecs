## Spec: POST /api/v1/auth/refresh

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/auth-refresh-post.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 저장된 refresh cookie 를 기반으로 현재 세션을 복구하거나  
> access token 을 재발급한다.

---

### 요구사항

- [x] request body 없이 호출할 수 있다.
- [x] 성공 시 사용자 정보와 새 access token 을 반환한다.
- [x] 유효한 refresh cookie 가 없으면 `null` 또는 401 응답을 반환할 수 있다.
- [x] cookie 기반 인증을 위해 `credentials: 'include'` 가 필요하다.

---

### 인터페이스 정의

```typescript
interface AuthUser {
  id: string
  name: string
  grade?: string
  subscription?: 'free' | 'premium' | 'family' | 'school'
}

interface AuthSessionResponse {
  user: AuthUser
  accessToken: string | null
}

type AuthRefreshResponse = AuthSessionResponse | null

interface ApiErrorResponse {
  code: string
  message: string
  details?: unknown
}
```

---

### 서버 검증 규칙

| 필드 | 규칙 |
|------|------|
| Cookie | 유효한 refresh cookie 가 있어야 한다. |

검증 실패 시 권장 에러:
- refresh cookie 없음: `401 REFRESH_SESSION_NOT_FOUND`
- refresh cookie 만료 또는 무효: `401 REFRESH_SESSION_INVALID`

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 유효한 refresh cookie 존재 | 새 세션 정보를 반환한다. |
| 세션 없음 | `null` 또는 인증 에러를 반환한다. |
| 앱 부트스트랩 | `restoreSession()` 에서 이 endpoint 를 사용한다. |
| 수동 세션 갱신 | `refreshSession()` 에서 같은 endpoint 를 재사용한다. |

---

### Query / Adapter

- React Query query key 없이 auth service 에서 직접 사용한다.
- 현재 remote service 는 응답 DTO 를 그대로 `AuthSession | null` 로 사용한다.

---

### 테스트 시나리오

- [x] restoreSession 은 refresh endpoint 를 호출한다.
- [x] refreshSession 은 세션 존재 시 refresh endpoint 를 호출한다.
- [x] refreshSession 은 세션이 없으면 네트워크 호출 없이 `null` 을 반환한다.
- [ ] refresh cookie 가 무효하면 `401 REFRESH_SESSION_INVALID` 를 반환한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | auth refresh endpoint spec 추가 | Codex |
| 2026-03-28 | 서버 검증 규칙과 에러 코드 기준 추가 | Codex |
