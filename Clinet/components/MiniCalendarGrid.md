## Spec: MiniCalendarGrid

**타입**: `Component`
**위치**: `Clinet/components/MiniCalendarGrid.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> MiniCalendarGrid는 대시보드에서 현재 월의 날짜 흐름과 시험/과제 이벤트를 간단히 보여준다.
> 사용자는 오늘 날짜와 이벤트 날짜를 빠르게 확인하고 필요한 경우 상세 캘린더로 이동할 수 있다.

---

### 요구사항 (Requirements)

- [ ] 요일 라벨과 월간 날짜 그리드를 렌더링한다.
- [ ] 오늘 날짜를 강조 표시한다.
- [ ] 이벤트가 있는 날짜에 점 또는 마커를 표시한다.
- [ ] 빈 셀을 지원해 월 시작 위치를 맞춘다.
- [ ] 날짜 클릭 콜백을 제공한다.
- [ ] 선택 날짜 상태를 외부에서 제어할 수 있어야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
interface MiniCalendarEvent {
  id: string
  date: string
  title?: string
}

interface MiniCalendarGridProps {
  month: Date
  selectedDate?: Date
  events?: MiniCalendarEvent[]
  weekStartsOn?: 0 | 1
  onDateSelect?: (date: Date) => void
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 오늘 날짜 | 강조 배경과 대비 텍스트를 사용한다 |
| 이벤트 날짜 | 하단 점 마커를 표시한다 |
| 선택 날짜 | 선택 스타일을 추가로 적용할 수 있다 |
| 빈 셀 | 클릭 불가 상태로 렌더링한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 월 시작일이 일요일/월요일일 때 빈 셀 계산이 정확해야 한다.
- [ ] 이벤트가 여러 개여도 대시보드 축약형에서는 단일 점 마커만 보여준다.
- [ ] 오늘 날짜와 이벤트 날짜가 겹쳐도 시각 대비가 유지되어야 한다.
- [ ] 로컬 타임존 변환으로 날짜가 하루 밀리지 않도록 주의해야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 오늘 날짜가 강조 표시된다.
- [ ] 이벤트 날짜에 마커가 표시된다.
- [ ] 빈 셀이 필요한 월에서 그리드 정렬이 정확하다.
- [ ] 날짜 클릭 시 `onDateSelect`가 호출된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | MiniCalendarGrid 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 날짜 표시 기준 확인
- [ ] 이벤트 마커 방식 확인
