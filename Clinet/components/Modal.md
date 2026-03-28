## Spec: Modal

**타입**: `Component`
**위치**: `Clinet/components/Modal.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> Modal은 추천 마법사, 자동 계획 생성, 결제, 확인 대화상자 등 오버레이 기반 UI의 공통 기반이다.
> 포커스 트랩, ESC 닫기, 배경 클릭 닫기 같은 기본 접근성과 상호작용 규칙을 제공한다.

---

### 요구사항 (Requirements)

- [ ] open 상태에 따라 오버레이와 콘텐츠를 렌더링한다.
- [ ] 제목, 본문, footer 액션 조합을 지원한다.
- [ ] ESC 키 닫기와 오버레이 클릭 닫기를 지원할 수 있어야 한다.
- [ ] 포커스 트랩과 초기 포커스 이동을 지원한다.
- [ ] body scroll lock을 제공한다.
- [ ] 작은 화면에서도 안전하게 표시되어야 한다.
- [ ] 앱 루트 바깥 포털 렌더링 정책을 가진다.
- [ ] 다른 overlay UI와 충돌하지 않는 z-index 규칙을 가진다.

---

### 인터페이스 정의 (Interface)

```typescript
import type { ReactNode } from 'react'

interface ModalProps {
  open: boolean
  title?: string
  description?: string
  children: ReactNode
  footer?: ReactNode
  onClose: () => void
  closeOnOverlayClick?: boolean
  closeOnEscape?: boolean
  initialFocusRef?: React.RefObject<HTMLElement>
  size?: 'sm' | 'md' | 'lg' | 'xl'
  portalTarget?: HTMLElement | null
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `open=true` | 오버레이와 모달 콘텐츠를 렌더링한다 |
| ESC 입력 | `closeOnEscape`가 true면 `onClose`를 호출한다 |
| 오버레이 클릭 | `closeOnOverlayClick`이 true면 닫는다 |
| 열릴 때 | 포커스를 모달 내부로 이동한다 |
| 닫힐 때 | 이전 포커스 위치 복원을 시도한다 |
| 렌더링 위치 | 기본적으로 앱 전역 portal root에 마운트한다 |

---

### 접근성 기준

- [ ] `role="dialog"` 또는 `alertdialog`를 제공한다.
- [ ] `aria-modal="true"`를 제공한다.
- [ ] 제목과 설명이 있을 경우 적절한 aria 연결을 제공한다.
- [ ] 키보드만으로 닫기와 주요 액션 수행이 가능해야 한다.

### 레이어 정책

- [ ] Modal은 기본적으로 Toast보다 낮지 않은 독립 overlay 레이어를 사용한다.
- [ ] Sidebar, sticky header, dropdown 위에 안정적으로 떠야 한다.
- [ ] 기본 portal root는 앱 최상단에 단일 노드로 둔다.

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 긴 콘텐츠는 내부 스크롤을 허용해야 한다.
- [ ] 중첩 모달은 기본적으로 지양하고, 필요 시 관리 정책이 필요하다.
- [ ] 모바일 키보드가 열릴 때 레이아웃이 과도하게 깨지지 않아야 한다.
- [ ] 스크롤 컨테이너 내부에서 잘리지 않도록 portal 사용이 사실상 기본값이어야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] open 상태에서 모달이 렌더링된다.
- [ ] ESC 입력으로 닫힌다.
- [ ] overlay 클릭으로 닫힌다.
- [ ] 포커스가 모달 내부에 유지된다.
- [ ] title과 description이 aria 속성으로 연결된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Modal 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 닫기 정책 확인
- [ ] size 프리셋 확인
