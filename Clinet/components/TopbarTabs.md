## Spec: TopbarTabs

**타입**: `Component`
**위치**: `Clinet/components/TopbarTabs.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> TopbarTabs는 대시보드 상단의 기간 전환 UI를 제공한다.
> 일간, 주간, 월간 같은 비교 기준을 빠르게 바꾸고 active 상태를 명확하게 보여준다.

---

### 요구사항 (Requirements)

- [ ] pill 형태의 탭 UI를 제공한다.
- [ ] `daily`, `weekly`, `monthly` 값을 지원한다.
- [ ] 현재 active 값이 시각적으로 구분되어야 한다.
- [ ] 탭 전환 시 상위 상태 변경 콜백을 호출한다.
- [ ] 모바일에서도 터치 가능한 최소 클릭 영역을 유지한다.

---

### 인터페이스 정의 (Interface)

```typescript
type DashboardPeriod = 'daily' | 'weekly' | 'monthly'

interface TopbarTabsOption {
  value: DashboardPeriod
  label: string
}

interface TopbarTabsProps {
  value: DashboardPeriod
  options?: TopbarTabsOption[]
  onChange: (value: DashboardPeriod) => void
  disabled?: boolean
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| active 탭 | surface 배경과 강조 텍스트로 표시한다 |
| 탭 클릭 | 해당 값을 상위에 전달한다 |
| disabled | 클릭을 막고 낮은 대비로 표시한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 라벨 길이는 짧은 한글 단어 기준으로 유지한다.
- [ ] 컨테이너 폭이 좁아도 탭이 줄바꿈되지 않아야 한다.
- [ ] 빠른 연속 클릭 시 상태가 꼬이지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] active 값이 바뀌면 스타일도 함께 변경된다.
- [ ] `onChange`가 클릭한 값으로 호출된다.
- [ ] disabled 상태에서는 이벤트가 발생하지 않는다.
- [ ] 모바일 폭에서도 탭 UI가 깨지지 않는다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | TopbarTabs 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 탭 종류 확인
- [ ] active 스타일 방향 확인
