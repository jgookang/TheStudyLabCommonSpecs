## Spec: Auth Session Lifecycle

**유형**: `Backend Design`  
**위치**: `Server/backend/auth-session-lifecycle.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 하나의 백엔드가 웹 프론트와 모바일 앱을 모두 지원할 수 있도록
> 공통 인증 코어와 웹/모바일 전송 방식을 분리해 정의한다.

---

### 관련 공용 스펙

- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-web-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-login-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-refresh-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\auth-mobile-logout-post.md`
- `C:\Users\jgook\repo\CommonSpecs\Common\api\endpoint-priority.md`

---

### 핵심 원칙

- 백엔드 인증 코어는 하나로 유지한다.
- access token, 사용자 식별, 세션 폐기, 권한 컨텍스트는 웹/모바일이 공통으로 사용한다.
- 웹과 모바일은 refresh transport 만 분리한다.
- 웹은 same-site 배포를 기본 전제로 한다.
- 웹 refresh 실패는 항상 명시적 `401` 로 응답한다.
- 모바일 refresh 는 secure storage 에 저장한 refresh token rotation 을 사용한다.

---

### 공통 세션 코어

#### Access token

- 로그인 또는 refresh 성공 시 access token 을 발급한다.
- 권장 기본 수명: `15분`
- 보호 endpoint 는 access token 또는 서버 인증 컨텍스트를 통해 보호한다.

#### Refresh session

- refresh token 은 항상 회전(rotation)한다.
- 서버는 refresh token 원문을 저장하지 않고 hash 만 저장한다.
- 권장 기본 수명: `30일`
- 폐기된 refresh token 은 재사용되면 즉시 `401` 로 처리한다.

#### 권장 저장 필드

```typescript
type AuthClientType = 'web' | 'mobile'

interface AuthSessionRecord {
  id: string
  userId: string
  clientType: AuthClientType
  refreshTokenHash: string
  createdAt: string
  expiresAt: string
  lastSeenAt: string | null
  revokedAt: string | null
  revokedReason: string | null
  userAgent: string | null
  ipAddress: string | null
  deviceId: string | null
  deviceLabel: string | null
}
```

---

### 웹 인증 전송 방식

#### 기본 정책

- 웹은 same-site 배포를 기본값으로 한다.
- refresh token 은 HttpOnly cookie 로만 보관한다.
- refresh cookie 는 JS 에서 읽을 수 없어야 한다.
- `POST /api/v1/auth/web/refresh` 는 세션 없음과 무효 세션 모두 `401` 로 반환한다.
- 웹의 상태 변경 auth endpoint 는 `Origin` 검사를 수행한다.

#### Cookie 정책

| 항목 | 권장값 | 메모 |
|------|--------|------|
| Name | `studypath_web_refresh_token` | 웹 전용 cookie |
| HttpOnly | `true` | JS 접근 금지 |
| Secure | 운영 `true` | 로컬 HTTP 개발은 환경별 예외 허용 |
| SameSite | `Lax` | Phase 1 기본값 |
| Path | `/api/v1/auth/web` | 웹 auth 하위로 제한 |
| Max-Age | refresh TTL 과 동일 | 세션 수명과 일치 |

#### 개발 / 배포 메모

- 프론트와 백엔드는 같은 상위 사이트에서 배포하는 것을 기본으로 한다.
- 개발 시 `localhost` 와 `127.0.0.1` 혼용은 피한다.
- `credentials: include` 를 사용하므로 CORS origin 은 명시 allowlist 로 관리한다.
- truly cross-site cookie 가 필요해지면 별도 보안 검토 후 다시 문서화한다.

#### 웹 endpoint

##### 1. `POST /api/v1/auth/web/login`

1. `email`, `password` payload 를 검증한다.
2. 사용자 검증 후 `clientType='web'` 세션을 생성한다.
3. refresh cookie 를 설정한다.
4. `user`, `accessToken` 을 응답 본문에 반환한다.

##### 2. `POST /api/v1/auth/web/refresh`

1. refresh cookie 를 읽는다.
2. 세션 존재, 만료, 폐기 여부를 검증한다.
3. 성공 시 refresh token 을 회전한다.
4. 새 cookie 와 새 `accessToken` 을 반환한다.

실패 정책:
- refresh cookie 없음: `401 REFRESH_SESSION_NOT_FOUND`
- refresh cookie 무효 또는 만료: `401 REFRESH_SESSION_INVALID`

##### 3. `POST /api/v1/auth/web/logout`

1. refresh cookie 가 있으면 세션을 revoke 처리한다.
2. cookie 를 제거한다.
3. 세션이 없어도 멱등 성공 처리한다.

권장 응답:
- `204 No Content`

---

### 모바일 인증 전송 방식

#### 기본 정책

- 모바일은 refresh token 을 앱 secure storage 에 저장한다.
- 앱은 refresh token 원문을 서버에 전달해 rotation 을 수행한다.
- 웹 cookie 정책은 모바일에 적용하지 않는다.
- 모바일도 같은 인증 코어와 같은 access token 검증 계층을 사용한다.

#### 모바일 endpoint

##### 1. `POST /api/v1/auth/mobile/login`

요청 본문:

```typescript
interface AuthMobileLoginRequest {
  email: string
  password: string
  deviceId?: string
  deviceLabel?: string
}
```

성공 응답:

```typescript
interface AuthMobileSessionResponse {
  user: {
    id: string
    name: string
    grade?: string
    subscription?: 'free' | 'premium' | 'family' | 'school'
  }
  accessToken: string
  refreshToken: string
}
```

##### 2. `POST /api/v1/auth/mobile/refresh`

요청 본문:

```typescript
interface AuthMobileRefreshRequest {
  refreshToken: string
  deviceId?: string
}
```

동작:
- refresh token 검증
- 세션 회전
- 새 `accessToken`, 새 `refreshToken`, `user` 반환

실패 정책:
- refresh token 없음: `401 REFRESH_SESSION_NOT_FOUND`
- refresh token 무효 또는 만료: `401 REFRESH_SESSION_INVALID`

##### 3. `POST /api/v1/auth/mobile/logout`

요청 본문:

```typescript
interface AuthMobileLogoutRequest {
  refreshToken: string
}
```

동작:
- refresh session revoke
- 멱등 성공

권장 응답:
- `204 No Content`

---

### 공통 인증 컨텍스트

- 보호 endpoint 는 인증 컨텍스트가 없으면 `401 AUTH_REQUIRED` 를 반환한다.
- 인증 미들웨어는 최소한 아래 정보를 downstream 에 전달해야 한다.

```typescript
interface AuthContext {
  userId: string
  sessionId?: string
  clientType: 'web' | 'mobile'
  roles?: string[]
}
```

- ownership, moderation, premium gate 는 인증 이후 별도 authorization 단계에서 처리한다.

---

### 보안 및 운영 메모

- refresh token 은 항상 hash 비교로 검증한다.
- 로그인 실패는 rate limit 또는 throttling 정책과 연결한다.
- 비밀번호 변경, 강제 로그아웃, 계정 제재 시 기존 세션을 일괄 폐기할 수 있어야 한다.
- auth 관련 로그에는 token 원문, 비밀번호, 민감 cookie 값을 남기지 않는다.
- 최소 이벤트 로그:
  - login success / failure
  - refresh success / invalid / expired
  - logout success
  - session revoked

---

### 테스트 기준

#### 공통 단위 테스트

- payload validation
- credential 검증 실패
- session 생성 및 revoke 로직
- refresh rotation 로직

#### 웹 통합 테스트

- login 성공 시 cookie 설정과 응답 본문 확인
- refresh 성공 시 새 access token 과 새 cookie 확인
- refresh 실패 시 명시적 `401` code 확인
- logout 멱등 성공 확인
- 잘못된 `Origin` 또는 허용되지 않은 origin 차단 확인

#### 모바일 통합 테스트

- login 성공 시 `accessToken`, `refreshToken` 동시 반환 확인
- refresh 성공 시 token rotation 확인
- refresh token 무효 시 `401` 확인
- logout 멱등 성공 확인

#### 웹 smoke 체크리스트

- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `POST /api/v1/auth/web/logout`
- 로그인 후 새로고침 시 세션 복구
- 세션 만료 시 프론트가 `ready + isAuthenticated=false` 로 안정적으로 정리되는지 확인

---

### 남은 결정 사항

- access token 형식: JWT vs opaque token
- refresh session 저장소: DB 단일 저장 vs cache + DB 혼합
- 모바일 device identifier 를 필수로 둘지 여부
- 사용자 role 모델과 moderation 제재 모델의 결합 방식

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 웹/모바일 분리형 인증 세션 설계로 개편 | Codex |
