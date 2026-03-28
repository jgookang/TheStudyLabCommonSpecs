## Spec: plans-today-get

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/plans-today-get.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 오늘의 학습 계획 목록을 조회한다.  
> 대시보드의 `오늘의 학습 계획` 영역에서 과목, 교재, 세부 내용, 예상 시간, 상태를 표시하기 위해 사용한다.

---

### 요구사항

- [x] 오늘 날짜 기준 계획 배열을 반환한다.
- [x] 각 항목은 과목, 교재, 세부 내용, 예상 시간, 상태, 색상 정보를 포함한다.
- [x] 인증이 필요하다.

---

### 인터페이스 정의

```typescript
type PlanStatus = 'pending' | 'in_progress' | 'done'

interface PlanItem {
  id: string
  subject: string
  book: string
  detail: string
  estimatedMin: number
  status: PlanStatus
  color: string
}

interface PlansTodayGetResponse {
  data: PlanItem[]
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
| Authorization | 필수. 유효한 access token 또는 서버 인증 컨텍스트가 있어야 한다. |
| `status` | 응답 값은 `pending`, `in_progress`, `done` 중 하나여야 한다. |
| `estimatedMin` | 1 이상의 정수여야 한다. |

검증 실패 시 권장 에러:
- 인증 없음: `401 AUTH_REQUIRED`
- 잘못된 상태 값: `500 INVALID_PLAN_STATUS`
- 잘못된 시간 값: `500 INVALID_PLAN_DURATION`

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 계획 존재 | 정렬된 계획 배열을 반환한다. |
| 계획 없음 | 빈 배열을 반환한다. |
| UI 매핑 | `subject + book` 은 title 로 변환하고 `estimatedMin` 은 분 문자열로 변환한다. |

---

### Query Key / Adapter

- React Query key: `['dashboard', 'plans-today-get', mode]`
- remote DTO 는 `data: PlanItem[]` 구조를 사용한다.
- UI 모델 변환 규칙:
  - `title = \`\${subject} - \${book}\``
  - `time = \`\${estimatedMin}분\``

---

### 엣지 케이스

- [x] 계획이 없으면 빈 배열을 반환한다.
- [ ] 색상 값이 비정상이면 기본 과목 색상 매핑 규칙이 필요하다.
- [ ] 사용자 로컬 날짜와 서버 UTC 날짜 차이를 고려해야 한다.

---

### 테스트 시나리오

- [x] 계획 목록을 배열로 반환한다.
- [x] 계획이 없을 때 빈 배열을 반환한다.
- [x] remote service 가 DTO 를 대시보드 카드 모델로 변환한다.
- [ ] 인증 없는 요청은 `401 AUTH_REQUIRED` 를 반환한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 오늘 계획 조회 API spec 초안 작성 | Codex |
| 2026-03-28 | React Query key 와 UI adapter 변환 규칙 반영 | Codex |
| 2026-03-28 | 서버 검증 규칙과 에러 코드 기준 추가 | Codex |
