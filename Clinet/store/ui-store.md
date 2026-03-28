## Spec: uiStore

**타입**: `Store Slice`
**위치**: `Clinet/store/ui-store.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> `uiStore`는 전역 UI 상태를 관리한다.
> 토스트, 모달 열림 여부, 전역 오버레이 같은 서버 데이터와 무관한 브라우저 상태를 일관되게 다룬다.

---

### 요구사항 (Requirements)

- [ ] 토스트 enqueue / dismiss / clear 기능을 제공한다.
- [ ] 전역 모달의 열림 상태와 식별자를 관리할 수 있어야 한다.
- [ ] 서버 데이터는 저장하지 않는다.
- [ ] 여러 화면에서 공유되는 UI 상태만 포함한다.

---

### 인터페이스 정의 (Interface)

```typescript
type ToastTone = 'success' | 'info' | 'warning' | 'error'

interface ToastItem {
  id: string
  tone: ToastTone
  message: string
  description?: string
  duration?: number
  actionLabel?: string
}

interface ModalState {
  id: string | null
  props?: Record<string, unknown>
}

interface UiStoreState {
  toasts: ToastItem[]
  modal: ModalState
  maxVisibleToasts?: number
}

interface UiStoreActions {
  pushToast: (toast: Omit<ToastItem, 'id'> & { id?: string }) => void
  dismissToast: (id: string) => void
  clearToasts: () => void
  openModal: (id: string, props?: Record<string, unknown>) => void
  closeModal: () => void
}

type UiStore = UiStoreState & UiStoreActions
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 토스트 추가 | 새 토스트를 배열 끝에 추가한다 |
| 토스트 닫기 | 해당 id 항목만 제거한다 |
| 모달 열기 | `modal.id`와 `props`를 갱신한다 |
| 모달 닫기 | `modal.id`를 `null`로 되돌린다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 동일 메시지 토스트의 중복 허용 정책이 필요하다.
- [ ] 너무 많은 토스트가 쌓일 경우 최대 개수 제한이 필요할 수 있다.
- [ ] `props`는 직렬화 가능한 데이터 위주로 유지하는 것이 바람직하다.
- [ ] Toast 컴포넌트가 요구하는 설명문/액션과 store shape가 일치해야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 토스트 추가 시 배열에 항목이 생성된다.
- [ ] 특정 토스트만 제거할 수 있다.
- [ ] 모달 열기 후 id와 props가 반영된다.
- [ ] 모달 닫기 시 상태가 초기화된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | uiStore Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 토스트 정책 확인
- [ ] 전역 모달 관리 범위 확인
