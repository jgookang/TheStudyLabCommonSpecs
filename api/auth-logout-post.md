## Spec: POST /api/v1/auth/logout

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/auth-logout-post.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 현재 사용자 세션을 종료하고 refresh cookie 를 무효화한다.

---

### 요구사항

- [x] request body 없이 호출할 수 있다.
- [x] 성공 시 본문이 없거나 단순 성공 응답을 반환할 수 있다.
- [x] cookie 기반 인증을 위해 `credentials: 'include'` 가 필요하다.

---

### 인터페이스 정의

```typescript
type AuthLogoutResponse = void

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
| Cookie | refresh cookie 가 있으면 제거한다. 없어도 멱등 성공 처리할 수 있다. |

권장 결과:
- 정상 로그아웃: `204 No Content` 또는 단순 성공 응답
- 세션 없음: 동일하게 성공 처리 가능

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 정상 로그아웃 | refresh cookie 를 제거하고 성공 응답을 반환한다. |
| 이미 세션 없음 | 멱등적으로 성공 처리할 수 있다. |
| 앱 로그아웃 버튼 | `remoteAuthService.logout()` 이 이 endpoint 를 호출한다. |

---

### Query / Adapter

- React Query query key 없이 auth service 에서 직접 사용한다.
- 현재 remote service 는 응답 본문을 사용하지 않는다.

---

### 테스트 시나리오

- [x] logout endpoint 로 POST 요청을 보낸다.
- [ ] 세션이 없는 상태에서도 멱등 성공 여부를 확인한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | auth logout endpoint spec 추가 | Codex |
| 2026-03-28 | 서버 검증 규칙과 멱등 처리 기준 추가 | Codex |
