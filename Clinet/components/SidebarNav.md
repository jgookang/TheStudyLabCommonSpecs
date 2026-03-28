## Spec: SidebarNav

**타입**: `Component`
**위치**: `Clinet/components/SidebarNav.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> SidebarNav는 StudyPath의 좌측 네비게이션 영역을 담당한다.
> 섹션 라벨, 메뉴 아이템, 활성 상태, badge, 사용자 요약 영역을 일관된 구조로 제공한다.

---

### 요구사항 (Requirements)

- [ ] 섹션 단위(`메인`, `성장`, `커뮤니티`) 그룹을 렌더링한다.
- [ ] 각 아이템은 label, active 상태, 클릭 동작을 가진다.
- [ ] 일부 아이템은 badge를 표시할 수 있다.
- [ ] active 아이템은 별도 배경과 텍스트 색상을 사용한다.
- [ ] 하단에는 현재 사용자 요약 카드 슬롯을 제공한다.
- [ ] 키보드 포커스와 hover 상태를 제공한다.

---

### 인터페이스 정의 (Interface)

```typescript
interface SidebarNavItem {
  id: string
  label: string
  href?: string
  active?: boolean
  badge?: string | number
  onClick?: () => void
}

interface SidebarNavSection {
  id: string
  label: string
  items: SidebarNavItem[]
}

interface SidebarUserSummary {
  name: string
  subtitle: string
  avatarText?: string
}

interface SidebarNavProps {
  sections: SidebarNavSection[]
  user?: SidebarUserSummary
  onNavigate?: (itemId: string) => void
  collapsed?: boolean
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| 메뉴 아이템 hover | 배경을 secondary 톤으로 강조한다 |
| 메뉴 아이템 active | `primary-light` 배경과 `primary-dark` 텍스트를 사용한다 |
| badge 존재 | 아이템 우측에 강조 badge를 표시한다 |
| collapsed 모드 | 텍스트를 축약하거나 tooltip 기반으로 대체한다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 섹션에 아이템이 0개일 때 섹션 자체를 숨길지 여부를 결정해야 한다.
- [ ] badge 텍스트가 길지 않도록 숫자 또는 짧은 문자열만 허용한다.
- [ ] 사용자 이름이 길어도 footer 레이아웃이 깨지지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] active 아이템 스타일이 정확히 적용된다.
- [ ] badge가 있는 메뉴와 없는 메뉴가 모두 정상 렌더링된다.
- [ ] collapsed 모드에서 레이아웃이 무너지지 않는다.
- [ ] 키보드 포커스로 아이템 이동이 가능하다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | SidebarNav 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 섹션 구조 확인
- [ ] footer 사용자 요약 영역 확인
