## Spec: Skeleton

**타입**: `Component`
**위치**: `Clinet/components/Skeleton.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> Skeleton은 데이터 로딩 중 실제 콘텐츠의 형태를 미리 보여주는 플레이스홀더다.
> 페이지 전체를 막지 않고 위젯 단위 로딩 경험을 부드럽게 만드는 데 사용한다.

---

### 요구사항 (Requirements)

- [ ] 너비와 높이를 지정할 수 있어야 한다.
- [ ] `text`, `rect`, `circle` 형태를 지원한다.
- [ ] shimmer 또는 subtle pulse 애니메이션을 지원할 수 있어야 한다.
- [ ] 단독 사용과 여러 개 조합 사용 모두 가능해야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
type SkeletonVariant = 'text' | 'rect' | 'circle'

interface SkeletonProps {
  width?: number | string
  height?: number | string
  variant?: SkeletonVariant
  animated?: boolean
  className?: string
}
```

---

### 동작 정의 (Behavior)

| 조건 | 동작 |
|------|------|
| `variant=text` | 본문 한 줄 같은 긴 직사각형 형태를 사용한다 |
| `variant=rect` | 카드/이미지 placeholder 형태를 사용한다 |
| `variant=circle` | 아바타/아이콘 placeholder 형태를 사용한다 |
| `animated=true` | 부드러운 로딩 애니메이션을 보여준다 |

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 너무 많은 skeleton이 동시에 애니메이션되면 성능에 부담이 될 수 있다.
- [ ] 부모 컨테이너 크기가 불안정할 때 layout shift를 만들지 않아야 한다.
- [ ] `prefers-reduced-motion` 환경에서 애니메이션 축소가 필요하다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] variant별 형태가 다르게 렌더링된다.
- [ ] width/height가 반영된다.
- [ ] animated 옵션이 반영된다.
- [ ] 원형 skeleton이 올바른 반경을 가진다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Skeleton 컴포넌트 Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] variant 종류 확인
- [ ] 애니메이션 강도 확인
