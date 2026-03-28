## Spec: dashboardStore

**타입**: `Store Slice`
**위치**: `Clinet/store/dashboard-store.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> `dashboardStore`는 대시보드의 로컬 UI 상태를 관리한다.
> 기간 탭, 선택 날짜 같은 서버 데이터와 분리된 화면 제어 상태를 보관한다.

---

### 요구사항 (Requirements)

- [ ] 대시보드 period 상태를 관리한다.
- [ ] 미니 캘린더 선택 날짜를 관리한다.
- [ ] 필요 시 위젯 접힘/펼침 같은 로컬 UI 상태를 확장 가능해야 한다.
- [ ] 서버 응답 데이터 자체는 저장하지 않는다.

---

### 인터페이스 정의 (Interface)

```typescript
type DashboardPeriod = 'daily' | 'weekly' | 'monthly'

interface DashboardStoreState {
  period: DashboardPeriod
  selectedDate: string | null
}

interface DashboardStoreActions {
  setPeriod: (period: DashboardPeriod) => void
  setSelectedDate: (date: string | null) => void
  resetDashboardUi: () => void
}

type DashboardStore = DashboardStoreState & DashboardStoreActions
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 탭 변경 | `period` 상태를 갱신한다 |
| 날짜 선택 | `selectedDate`를 ISO 문자열 기준으로 저장한다 |
| reset | 기본 상태(`daily`, `null`)로 되돌린다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 시간대에 따라 날짜 문자열이 어긋나지 않도록 날짜 저장 기준을 명확히 해야 한다.
- [ ] period 변경 시 관련 query key도 일관되게 따라가야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 기본값이 `daily`와 `null`이다.
- [ ] `setPeriod()`가 올바른 값을 반영한다.
- [ ] `setSelectedDate()`가 날짜 문자열을 저장한다.
- [ ] `resetDashboardUi()`가 기본 상태로 복귀시킨다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | dashboardStore Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 저장 범위 확인
- [ ] 날짜 표현 방식 확인
