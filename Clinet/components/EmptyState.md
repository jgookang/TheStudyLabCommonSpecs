## Spec: EmptyState

**타입**: `Component`
**위치**: `Clinet/components/EmptyState.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> EmptyState는 데이터가 없을 때 사용자에게 현재 상태와 다음 행동을 분명하게 안내한다.
> 대시보드의 계획 없음, 알림 없음, 캘린더 일정 없음 같은 상황에서 재사용된다.

---

### 요구사항 (Requirements)

- [ ] 아이콘, 제목, 설명, CTA를 표시할 수 있어야 한다.
- [ ] 카드 내부와 전체 섹션 모두에서 사용할 수 있어야 한다.
- [ ] 과도하게 비어 보이지 않도록 적절한 여백과 정렬을 제공한다.
- [ ] CTA는 선택 사항이어야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
import type { ReactNode } from 'react'

interface EmptyStateProps {
  icon?: ReactNode
  title: string
  description?: string
  action?: ReactNode
  align?: 'left' | 'center'
  compact?: boolean
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| compact 모드 | 더 작은 여백과 텍스트 밀도를 사용한다 |
| action 존재 | 제목/설명 아래 CTA를 배치한다 |
| align=center | 아이콘과 텍스트를 중앙 정렬한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 설명이 길어도 과도하게 장문이 되지 않도록 제한이 필요하다.
- [ ] CTA가 여러 개 필요한 경우 별도 정책이 필요하다.
- [ ] 너무 작은 카드에서도 아이콘과 텍스트가 답답하게 보이지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] title 필수 렌더링이 된다.
- [ ] description과 action이 선택적으로 표시된다.
- [ ] compact 모드가 다른 spacing을 적용한다.
- [ ] 중앙 정렬 모드가 정상 동작한다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | EmptyState 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 기본 정렬 방향 확인
- [ ] compact 사용 범위 확인
