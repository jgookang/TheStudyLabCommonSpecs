## Spec: API Client Wrapper

**유형**: `API Endpoint`  
**위치**: `Clinet/api/client-wrapper.md`  
**작성일**: 2026-03-28  
**상태**: `Implemented`

---

### 목적

> 기능별 API 호출에서 fetch, 에러 파싱, mock 분기, 응답 변환을 중복 구현하지 않도록  
> 공통 client wrapper와 service / adapter 분리 규칙을 제공한다.

---

### 요구사항

- [x] 공통 `ApiClientError` 타입을 제공한다.
- [x] `GET / POST / PATCH / DELETE` JSON 요청 helper를 제공한다.
- [x] mock 단계에서 `default / empty / error` 모드를 제어할 수 있어야 한다.
- [x] query string 생성 규칙은 공통화한다.
- [x] feature service는 `mock` 과 `remote` 구현을 분리해둘 수 있어야 한다.
- [x] remote service는 spec DTO를 UI 모델로 변환하는 adapter 계층을 둘 수 있어야 한다.
- [x] 인증 복구용 요청을 위해 기본 `credentials: 'include'` 를 지원한다.

---

### 인터페이스 정의

```typescript
class ApiClientError extends Error {
  code: string
  status: number
  details?: unknown
}

function buildApiUrl(
  path: `/api/${string}`,
  query?: Record<string, string | number | boolean | null | undefined>,
): string

function requestJson<T>(
  path: `/api/${string}`,
  init?: RequestInit & {
    query?: Record<string, string | number | boolean | null | undefined>
  },
): Promise<T>

function simulateApiResponse<T>(options: {
  data: T
  emptyData: T
  mode?: 'default' | 'empty' | 'error'
  delayMs?: number
  errorCode?: string
  errorMessage?: string
}): Promise<T>
```

---

### 계층 규칙

1. `shared/api/client.ts` 는 순수 HTTP wrapper와 mock 응답 유틸만 가진다.
2. feature API 는 `types / mock / remote / adapters / service / hooks` 레이어로 분리할 수 있다.
3. `mock` 구현은 현재 제품 동작과 테스트 기준선을 제공한다.
4. `remote` 구현은 `/api/v1` 백엔드 계약을 먼저 고정하는 역할을 한다.
5. 화면은 feature hooks만 사용하고, data source 분기는 service 레이어에서 숨긴다.
6. spec DTO와 UI 모델이 다르면 adapter에서 변환하고 화면에는 UI 모델만 노출한다.

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 정상 요청 | JSON payload를 파싱해 반환한다. |
| 비정상 응답 | `ApiClientError` 로 변환한다. |
| JSON 파싱 실패 | raw text 기반으로 안전하게 fallback 처리한다. |
| mock `empty` 모드 | `emptyData` 를 반환한다. |
| mock `error` 모드 | `ApiClientError` 를 throw 한다. |
| query 포함 요청 | `buildApiUrl()` 로 URL을 생성한다. |
| 인증 복구 요청 | 기본 `credentials: 'include'` 로 쿠키를 함께 보낸다. |

---

### 테스트 시나리오

- [x] query params가 포함된 URL이 올바르게 생성된다.
- [x] mock `default` / `empty` 응답이 각각 맞게 반환된다.
- [x] mock `error` 모드에서 `ApiClientError` 가 발생한다.
- [x] dashboard service가 `mock / remote` 소스를 올바르게 선택한다.
- [x] dashboard remote adapter가 spec DTO를 화면 모델로 변환한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 공통 API client wrapper와 mock 응답 규칙 추가 | Codex |
| 2026-03-28 | feature service 계층 분리 규칙과 dashboard data source 선택 구조 반영 | Codex |
| 2026-03-28 | response adapter 계층과 기본 credentials 정책 반영 | Codex |
