## Spec: Button

**타입**: `Component`
**위치**: `Clinet/components/Button.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> Button은 StudyPath 전역에서 사용하는 기본 액션 컴포넌트다.
> primary, outline, ghost 스타일과 로딩 상태, 아이콘 배치를 일관된 방식으로 제공한다.

---

### 요구사항 (Requirements)

- [ ] `primary`, `outline`, `ghost` variant를 지원한다.
- [ ] `sm`, `md`, `lg` size를 지원한다.
- [ ] `loading` 상태에서 중복 클릭을 막는다.
- [ ] `icon` 단독 또는 `icon + label` 구성을 지원한다.
- [ ] 키보드 포커스, disabled, hover 상태를 제공한다.
- [ ] 버튼 텍스트는 한 줄로 유지하는 것을 기본으로 한다.

---

### 인터페이스 정의 (Interface)

```typescript
import type { ButtonHTMLAttributes, ReactNode } from 'react'

type ButtonVariant = 'primary' | 'outline' | 'ghost'
type ButtonSize = 'sm' | 'md' | 'lg'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: ButtonVariant
  size?: ButtonSize
  loading?: boolean
  icon?: ReactNode
  iconPosition?: 'left' | 'right'
  fullWidth?: boolean
}
```

---

### 스타일 기준

| variant | 배경 | 텍스트 | 보더 |
|---------|------|--------|------|
| `primary` | `--color-primary` | white | `--color-primary` |
| `outline` | transparent | 기본 텍스트 | 중립 보더 |
| `ghost` | transparent | 기본 텍스트 | 없음 |

| size | 높이 느낌 | 패딩 | 폰트 |
|------|-----------|------|------|
| `sm` | 조밀함 | 작음 | `text-caption` |
| `md` | 기본 | 중간 | `text-body` |
| `lg` | 강조 | 큼 | `text-body` 이상 |

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| hover | variant에 맞는 강조 상태를 적용한다 |
| focus | 명확한 focus-visible 링을 표시한다 |
| disabled | 클릭 불가, 낮은 대비 스타일을 적용한다 |
| loading | spinner 또는 loading 표시를 보여주고 클릭을 막는다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 아이콘만 있는 버튼은 `aria-label`이 필요하다.
- [ ] 긴 텍스트는 모바일에서 레이아웃을 깨지 않도록 적절한 최소/최대 폭 정책이 필요하다.
- [ ] loading 상태에서 레이블 폭이 크게 흔들리지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] variant별 스타일 클래스가 정상 적용된다.
- [ ] loading 상태에서 클릭 이벤트가 막힌다.
- [ ] disabled 상태에서 포인터 상호작용이 비활성화된다.
- [ ] 아이콘 좌/우 배치가 정상 동작한다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Button 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] variant 범위 확인
- [ ] loading 처리 방식 확인
