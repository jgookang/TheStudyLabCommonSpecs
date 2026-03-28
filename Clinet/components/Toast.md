## Spec: Toast

**타입**: `Component`
**위치**: `Clinet/components/Toast.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> Toast는 저장 완료, 세션 완료, 오류, 안내 메시지 같은 짧은 피드백을 전역적으로 표시한다.
> `uiStore`와 연결되어 앱 어디서나 일관된 방식으로 사용자에게 즉시 상태 변화를 전달한다.

---

### 요구사항 (Requirements)

- [ ] `success`, `info`, `warning`, `error` 톤을 지원한다.
- [ ] 메시지와 선택적 설명 텍스트를 표시할 수 있어야 한다.
- [ ] 자동 닫힘과 수동 닫힘을 지원한다.
- [ ] 여러 토스트가 동시에 쌓일 수 있어야 한다.
- [ ] 화면 우측 하단 기본 배치를 지원한다.
- [ ] 라이브 리전 등 접근성 고려가 필요하다.

---

### 인터페이스 정의 (Interface)

```typescript
import type { ReactNode } from 'react'

type ToastTone = 'success' | 'info' | 'warning' | 'error'

interface ToastProps {
  id: string
  tone: ToastTone
  message: string
  description?: string
  duration?: number
  actionLabel?: string
  onAction?: (id: string) => void
  onClose?: (id: string) => void
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 렌더링 시 | tone에 맞는 색상과 아이콘 스타일을 사용한다 |
| duration 존재 | 지정 시간이 지나면 자동 닫힘을 요청한다 |
| 닫기 버튼 클릭 | `onClose(id)`를 호출한다 |
| 여러 개 존재 | 최근 항목이 시각적으로 우선순위를 가진다 |

---

### 접근성 기준

- [ ] 성공/정보 토스트는 `role="status"` 또는 적절한 live region을 사용한다.
- [ ] 오류 토스트는 `role="alert"` 사용을 검토한다.
- [ ] 자동 닫힘이 있는 경우 너무 짧지 않은 표시 시간을 제공한다.

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 메시지가 길어도 토스트 폭이 과도하게 커지지 않아야 한다.
- [ ] 동일한 토스트가 짧은 시간에 반복 생성될 때 중복 정책이 필요하다.
- [ ] 액션 버튼이 들어가도 닫기 버튼과 충돌하지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] tone별 스타일이 다르게 적용된다.
- [ ] 닫기 버튼 클릭 시 `onClose`가 호출된다.
- [ ] duration이 있을 때 자동 닫힘이 동작한다.
- [ ] description과 actionLabel이 있을 때 정상 렌더링된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Toast 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] tone 종류 확인
- [ ] 자동 닫힘 정책 확인
