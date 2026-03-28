## Spec: Card

**타입**: `Component`
**위치**: `Clinet/components/Card.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> Card는 대시보드와 각 기능 화면의 기본 표면 컨테이너다.
> 여백, 보더, hover 상태, 내부 헤더 구조의 일관성을 담당한다.

---

### 요구사항 (Requirements)

- [ ] 기본 흰색 표면과 얇은 보더 스타일을 제공한다.
- [ ] `padding` 프리셋을 지원한다.
- [ ] 선택적으로 `hover` 스타일을 제공한다.
- [ ] `asChild` 또는 유사 패턴 없이도 일반 wrapper로 충분히 사용할 수 있어야 한다.
- [ ] 헤더/본문/푸터 조합에 자연스럽게 대응해야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
import type { HTMLAttributes, ReactNode } from 'react'

type CardPadding = 'sm' | 'md' | 'lg' | 'none'

interface CardProps extends HTMLAttributes<HTMLDivElement> {
  padding?: CardPadding
  hover?: boolean
  children: ReactNode
}
```

---

### 스타일 기준

- [ ] 배경: `--color-surface`
- [ ] 보더: 중립 tertiary border
- [ ] 기본 반경: `radius-lg`
- [ ] 기본 패딩: `spacing-md`
- [ ] hover 시 약한 배경 강조 또는 shadow를 허용한다.

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `hover=true` | 포인터 hover 시 강조 효과를 보여준다 |
| `padding=none` | 내부 패딩 없이 콘텐츠를 감싼다 |
| 추가 className | 기본 스타일 위에 확장 가능해야 한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 내부 콘텐츠가 높아져도 보더와 라운드 처리가 어색하지 않아야 한다.
- [ ] 카드 안에 클릭 가능한 요소가 많을 때 hover 강조가 과도하지 않아야 한다.
- [ ] 배경색이 다른 부모 컨텍스트에서도 경계가 유지되어야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 기본 스타일이 정상 렌더링된다.
- [ ] padding 프리셋에 따라 내부 여백이 달라진다.
- [ ] hover 옵션이 있을 때만 강조 스타일이 적용된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Card 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] padding 프리셋 확인
- [ ] hover 강도 확인
