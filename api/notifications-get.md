## Spec: notifications-get

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/notifications-get.md`  
**작성일**: 2026-03-28  
**상태**: `Draft`

---

### 목적

> 대시보드 알림 섹션과 상단 알림 버튼에 필요한 최신 알림 목록을 조회한다.  
> 시험 일정, 목표 달성, 추천 업데이트 같은 요약 알림을 최신순으로 제공한다.

---

### 요구사항

- [x] 최신 알림 목록을 반환한다.
- [x] 각 알림은 제목, 설명, 시간 라벨, tone 정보를 가진다.
- [x] 대시보드에서는 최근 3개 기준으로 우선 사용한다.
- [x] 인증이 필요하다.

---

### 인터페이스 정의

```typescript
type NotificationTone = 'default' | 'success' | 'info' | 'warning'

interface NotificationItem {
  id: string
  title: string
  description: string
  timeLabel: string
  tone: NotificationTone
}

interface NotificationsGetResponse {
  data: NotificationItem[]
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

검증 실패 시 권장 에러:
- 인증 없음: `401 AUTH_REQUIRED`

---

### 동작 정의

| 조건 | 동작 |
|------|------|
| 기본 조회 | 최신 알림 배열을 반환한다. |
| 알림 없음 | 빈 배열을 반환한다. |
| 비정상 응답 | `ApiClientError` 로 변환된다. |

---

### Query Key / Adapter

- React Query key: `['dashboard', 'notifications-get', mode]`
- remote DTO 는 `data: NotificationItem[]` 구조를 사용하고 화면에서는 그대로 notification 모델로 사용한다.

---

### 엣지 케이스

- [x] 알림이 0개면 빈 배열을 반환한다.
- [ ] `timeLabel` 생성 주체는 서버 기준으로 최종 확정이 필요하다.
- [x] 인증 없는 요청은 401 에러를 반환해야 한다.

---

### 테스트 시나리오

- [x] 최신 알림 목록을 반환한다.
- [x] 알림이 없으면 빈 배열을 반환한다.
- [ ] 인증 없는 요청은 `401 AUTH_REQUIRED` 를 반환한다.
- [x] remote service 가 DTO wrapper 를 언랩한다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 알림 조회 API spec 초안 작성 | Codex |
| 2026-03-28 | React Query key 와 remote DTO 구조 기준 반영 | Codex |
| 2026-03-28 | 서버 검증 규칙과 에러 코드 기준 추가 | Codex |
