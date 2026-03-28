## Spec: POST /api/v1/auth/login

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/auth-login-post.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 이메일과 비밀번호로 사용자 세션을 생성하고  
> 앱에서 사용할 access token 과 사용자 정보를 반환한다.

---

### 요구사항

- [x] `email`, `password` payload를 받는다.
- [x] 성공 시 사용자 정보와 access token 을 반환한다.
- [x] 실패 시 인증 에러를 반환한다.

---

### 인터페이스 정의

```typescript
interface AuthLoginRequest {
  email: string
  password: string
}

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
| `email` | 필수. 비어 있지 않아야 하고 이메일 형식이어야 한다. |
| `password` | 필수. 비어 있지 않아야 한다. |

검증 실패 시 권장 에러:
- 잘못된 payload: `400 INVALID_CREDENTIALS_PAYLOAD`
- 잘못된 자격 증명: `401 INVALID_CREDENTIALS`

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 정상 로그인 | 사용자 정보와 access token 을 반환한다. |
| 잘못된 자격 증명 | 401 계열 인증 에러를 반환한다. |
| 서버 오류 | 공통 `ApiClientError` 로 변환된다. |

---

### Query / Adapter

- React Query mutation 에서 직접 사용하고 별도 query key 는 없다.
- 현재 remote service 는 응답 DTO 를 그대로 `AuthSession` 모델로 사용한다.

---

### 테스트 시나리오

- [x] login endpoint 로 credentials 를 POST 한다.
- [ ] 잘못된 자격 증명은 `401 INVALID_CREDENTIALS` 를 반환한다.
- [ ] 빈 이메일 또는 비밀번호 payload 는 `400 INVALID_CREDENTIALS_PAYLOAD` 를 반환한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | auth login endpoint spec 추가 | Codex |
| 2026-03-28 | 서버 검증 규칙과 에러 코드 기준 추가 | Codex |
