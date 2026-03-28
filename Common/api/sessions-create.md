## Spec: sessions-create

**타입**: `API Endpoint`
**위치**: `Common/api/sessions-create.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> 완료된 학습 세션을 저장한다.
> 타이머 완료 시 세션 기록, 과목, 소요 시간, 메모 등을 서버에 저장하고 포인트/기록 시스템과 연동한다.

---

### 요구사항 (Requirements)

- [ ] 학습 세션 생성 요청을 받는다.
- [ ] subject, durationSeconds, mode를 포함한다.
- [ ] 선택적으로 메모나 planId를 받을 수 있다.
- [ ] 성공 시 생성된 세션 식별자와 저장 시간을 반환한다.
- [ ] 인증이 필요하다.

---

### 인터페이스 정의 (Interface)

```typescript
type TimerMode = 'focus' | 'short' | 'long'

interface SessionsCreateRequest {
  subject: string
  durationSeconds: number
  mode: TimerMode
  completedAt: string
  planId?: string
  note?: string
}

interface SessionsCreateResponse {
  data: {
    id: string
    subject: string
    durationSeconds: number
    mode: TimerMode
    completedAt: string
    createdAt: string
  }
}

interface ApiErrorResponse {
  code: string
  message: string
  details?: unknown
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 정상 요청 | 세션을 저장하고 생성 결과를 반환한다 |
| planId 포함 | 계획과 세션을 연결한다 |
| 저장 성공 | 관련 metrics / history / points 캐시 무효화 대상이 된다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] `durationSeconds <= 0`은 validation error가 되어야 한다.
- [ ] 동일 세션의 중복 저장 방지 정책이 필요하다.
- [ ] 오프라인 복구 시 재전송 중복 처리 기준이 필요하다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 정상 payload로 세션이 생성된다.
- [ ] `planId` 포함 요청이 허용된다.
- [ ] 잘못된 duration 값은 실패한다.
- [ ] 인증 없는 요청은 실패한다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 세션 생성 API Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 저장 필드 확인
- [ ] 중복 방지 정책 확인
