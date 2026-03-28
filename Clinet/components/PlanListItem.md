## Spec: PlanListItem

**타입**: `Component`
**위치**: `Clinet/components/PlanListItem.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> PlanListItem은 오늘의 학습 계획 목록에서 개별 항목을 표시한다.
> 과목 색상, 완료 체크, 제목/세부 정보, 상태 및 예상 시간을 한 줄 구조 안에서 읽기 쉽게 전달한다.

---

### 요구사항 (Requirements)

- [ ] 좌측 컬러바로 과목 또는 카테고리 색상을 표시한다.
- [ ] 완료 상태를 토글할 수 있는 체크 UI를 제공한다.
- [ ] 제목, 세부 설명, 상태, 시간 정보를 표시한다.
- [ ] `done`, `in_progress`, `pending` 상태를 구분해 보여준다.
- [ ] 긴 텍스트에도 레이아웃이 유지되어야 한다.
- [ ] hover 상태를 제공한다.

---

### 인터페이스 정의 (Interface)

```typescript
type PlanStatus = 'pending' | 'in_progress' | 'done'

interface PlanListItemProps {
  id: string
  subject: string
  title: string
  detail: string
  estimatedMin: number
  color: string
  status: PlanStatus
  onToggle?: (id: string) => void
  onClick?: (id: string) => void
  disabled?: boolean
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `done` | 체크 원형을 채우고 완료 스타일을 적용한다 |
| `in_progress` | 상태 텍스트를 강조 색상으로 표시한다 |
| `pending` | 중립 상태 스타일을 사용한다 |
| 항목 hover | 배경을 secondary 톤으로 약하게 강조한다 |
| 체크 클릭 | 상위 토글 콜백을 호출한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 제목과 세부 정보가 모두 길 때도 우측 시간 영역이 밀리지 않아야 한다.
- [ ] `estimatedMin`이 0 또는 누락일 때 대체 문구 정책이 필요하다.
- [ ] 체크 클릭과 항목 전체 클릭의 이벤트 충돌을 방지해야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] `done`, `in_progress`, `pending` 상태별 스타일이 올바르게 표시된다.
- [ ] 체크 클릭 시 `onToggle`가 호출된다.
- [ ] 긴 텍스트가 들어와도 시간 영역이 유지된다.
- [ ] hover 상태가 적용된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | PlanListItem 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 상태 종류 확인
- [ ] 텍스트/시간 레이아웃 확인
