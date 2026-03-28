## Spec: Schedule Management

**유형**: `Feature`
**위치**: `Clinet/features/schedule-management.md`
**작성일**: 2026-03-28
**상태**: `In Progress`

---

### 목적

> 학습 계획을 주간/월간 일정 보드로 배치하고,
> 사용자가 주간 보드에서 일정을 직접 생성, 수정, 삭제, 이동하고 자동 계획 제안을 적용할 수 있게 한다.

---

### 범위

- [x] `/schedule`
- [x] 주간 일정 보드
- [x] 월간 일정 보드
- [x] 일정 생성 / 수정 / 삭제
- [x] drag-and-drop 이동
- [x] 자동 계획 제안 모달

---

### 현재 구현 기준

- [x] AppShell 보호 라우트 안에서 `/schedule` 진입 가능
- [x] `schedule-board-get` query hook 구현
- [x] mock / remote schedule service 구현
- [x] 주간 / 월간 전환 UI 구현
- [x] overview metric, 일정 컬럼, 인사이트 카드 구현
- [x] 일정 추가 모달 구현
- [x] 일정 수정 / 삭제 액션 구현
- [x] drag-and-drop 이동 구현
- [x] 자동 계획 제안 모달과 적용 액션 구현
- [x] optimistic update와 remote contract test 추가

---

### 이동 규칙

- 이동은 주간 보기에서만 지원한다.
- 세션 카드를 다른 요일 칼럼으로 드래그하면 같은 `schedule update` mutation으로 처리한다.
- 이동 시 시간, 제목, 상태는 유지하고 `columnId`만 바뀐다.

---

### 자동 계획 규칙

- 자동 계획 제안은 현재 주간 보드의 세션 밀도와 과목 분포를 기준으로 생성한다.
- 추천 세션은 `schedule create` mutation을 반복 호출해 적용한다.
- 적용된 세션은 `originLabel = 자동 배치`로 표시한다.

---

### 남은 구현

- [ ] 일정 상세 메모, 우선순위 같은 확장 필드 검토
- [ ] 실제 백엔드 payload 기준 validation 규칙 확정
- [ ] 추천 규칙을 실제 서버 기반 추천으로 교체할지 결정

---

### 구현 메모

- 월간 보기는 현재 읽기 전용 요약으로 유지한다.
- 생성/수정/삭제/이동/자동 계획 적용은 주간 보기에서만 허용한다.
