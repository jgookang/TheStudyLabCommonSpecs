## Spec: StudyPath Project Foundation

**타입**: `Feature`
**위치**: `Clinet/features/project-foundation.md`
**작성일**: 2026-03-28
**상태**: `Draft`
**참조 문서**: `StudyPath_프론트엔드_개발_스펙_v1.0.docx`

---

### 목적 (Purpose)

> StudyPath는 자기주도 학습 로드맵 플랫폼이며, 대시보드·커리큘럼·스케줄·습관 관리·게이미피케이션·커뮤니티·수익화 기능을 단계적으로 확장한다.
> 본 Spec은 `StudyPath_프론트엔드_개발_스펙_v1.0` 문서를 기준으로 프론트엔드 전체 기반 구조와 선행 구현 범위를 정리하는 프로젝트 기준 문서다.
> 이후 각 Phase별 세부 구현은 별도 Spec으로 분리한다.

---

### 요구사항 (Requirements)

#### 기능 요구사항
- [ ] Phase 0에서 공통 UI, 디자인 토큰, 라우터 구조, 상태 관리 기반을 먼저 구축한다.
- [ ] Phase 1에서 대시보드 핵심 UI 7개 위젯과 타이머 로직을 제공한다.
- [ ] Phase 2에서 커리큘럼, 교재 DB, AI 추천, 스케줄 캘린더를 구현한다.
- [ ] Phase 3에서 학습 기록, 차트, 습관 관리, 레벨/배지 시스템을 구현한다.
- [ ] Phase 4에서 커뮤니티, 프리미엄 게이트, 결제, 제휴 링크, 광고 슬롯을 구현한다.
- [ ] 공통 컴포넌트는 Phase별 기능에서 재사용 가능하도록 shared 계층에 배치한다.
- [ ] API 연동 규약, 에러 포맷, 페이지네이션, 인증 방식을 전역 일관성 있게 유지한다.

#### 비기능 요구사항
- [ ] React 18 + TypeScript 기반으로 타입 안전성을 유지한다.
- [ ] Tailwind CSS와 CSS 변수 기반 디자인 토큰 시스템을 적용한다.
- [ ] 서버 상태와 클라이언트 상태의 책임을 React Query와 Zustand로 분리한다.
- [ ] 접근성은 WCAG 2.1 AA 수준을 목표로 한다.
- [ ] 핵심 대시보드 페이지는 Lighthouse 성능 85 이상, 접근성 90 이상을 목표로 한다.
- [ ] 모든 주요 화면은 모바일, 태블릿, 데스크톱 브레이크포인트에 대응한다.
- [ ] 애니메이션은 `prefers-reduced-motion` 환경을 고려해 축소 가능해야 한다.

---

### 기술 스택 (Tech Stack)

#### 프레임워크 결정

- [ ] 권장안: `Vite 5 + React Router v6`
- [ ] 권장 이유:
  - 현재 정의된 범위가 로그인 후 대시보드 중심의 프론트엔드 제품이며, 초기 Phase에서 SSR/SEO 이점이 핵심 요구로 보이지 않는다.
  - 디자인 시스템, 위젯, 상태관리, 인터랙션을 빠르게 고정하는 데 Vite 개발 루프가 유리하다.
  - `StudyPath_프론트엔드_개발_스펙_v1.0` 원문이 이미 React/Vite/React Router 조합을 전제로 작성되어 있다.
- [ ] 단, 아래 조건이 핵심 요구로 바뀌면 `Next.js App Router` 재검토가 필요하다.
  - 마케팅 랜딩 페이지 SEO가 최우선
  - 서버 렌더링 기반 초기 성능/OG/meta 전략이 중요
  - BFF나 서버 액션 중심 아키텍처를 강하게 원함

#### 선택 결과

> 현재 Spec의 권장 기준은 `Vite 5 + React Router v6`로 둔다.
> 구현은 이 기준을 우선 적용하되, 추후 제품 요구가 바뀌면 App Router 전환 여부를 별도 ADR로 검토한다.

| 영역 | 기술 | 선택 근거 |
|------|------|-----------|
| 프레임워크 | React 18 + TypeScript | 타입 안전성, 장기 확장성 |
| 번들러 | Vite 5 | 빠른 HMR, ESM 네이티브 지원 |
| 스타일 | Tailwind CSS v3 | 유틸리티 퍼스트, 토큰 시스템 연계 |
| 상태관리 | Zustand + React Query | 클라이언트/서버 상태 분리 |
| 라우팅 | React Router v6 | 중첩 라우트, lazy loading |
| 폼 | React Hook Form + Zod | 타입 안전 검증 |
| 차트 | Recharts | React 친화적 커스터마이징 |
| 테스트 | Vitest + Testing Library | Vite 친화적 테스트 환경 |
| 아이콘 | Lucide React | 일관된 SVG 아이콘 시스템 |

---

### 인터페이스 정의 (Interface)

```typescript
type AppRoute =
  | '/dashboard'
  | '/curriculum'
  | '/curriculum/:id'
  | '/schedule'
  | '/habit'
  | '/analytics'
  | '/community'
  | '/settings'

type AppPhase = 'phase-0' | 'phase-1' | 'phase-2' | 'phase-3' | 'phase-4'

interface PhaseSpec {
  id: AppPhase
  name: string
  goal: string
  deliverables: string[]
  prerequisites?: AppPhase[]
}

interface AppPageSpec {
  path: AppRoute
  componentName: string
  purpose: string
  phase: AppPhase
  authRequired: boolean
}

interface SharedComponentSpec {
  name: string
  location: `src/shared/components/${string}`
  phase: AppPhase
  priority: 'P0' | 'P1' | 'P2'
  purpose: string
}

interface StoreSpec {
  name: string
  location: `src/shared/stores/${string}.ts`
  kind: 'ui' | 'session' | 'auth' | 'gamification'
  persisted?: boolean
  serverSync?: boolean
}

interface ApiContractSpec {
  method: 'GET' | 'POST' | 'PATCH' | 'DELETE'
  path: `/api/v1/${string}`
  cacheStrategy: string
  authRequired: boolean
  optimistic?: boolean
}
```

---

### 디자인 토큰 (Design Tokens)

#### 컬러 팔레트

| 토큰 | 값 | 용도 | 사용 위치 |
|------|----|------|----------|
| `--color-primary` | `#1D9E75` | 메인 브랜드 색상 | 버튼, 링크, 진행바 |
| `--color-primary-dark` | `#0F6E56` | 강조, hover 상태 | 네비게이션 활성 |
| `--color-primary-light` | `#E1F5EE` | 배경 강조 | 카드 배경, 태그 |
| `--color-secondary` | `#378ADD` | 보조 정보 색상 | 링크, 과목 강조 |
| `--color-warning` | `#EF9F27` | 경고, 일정 | 캘린더 이벤트 |
| `--color-danger` | `#E24B4A` | 오류, 긴급 상태 | 에러, 알림 뱃지 |
| `--color-surface` | `#FFFFFF` | 카드 표면 | 카드 전반 |
| `--color-bg` | `#F1EFE8` | 페이지 배경 | 앱 배경 |

#### 시맨틱 토큰

| 토큰 | 권장 값 | 용도 |
|------|---------|------|
| `--color-text-primary` | `#1F241F` | 기본 본문, 제목 |
| `--color-text-secondary` | `#5F665F` | 보조 설명, 메타 정보 |
| `--color-text-tertiary` | `#8B918A` | 약한 라벨, 날짜 헤더 |
| `--color-border-primary` | `#D8D6CF` | 강조 보더 |
| `--color-border-secondary` | `#E3E0D8` | 일반 인터랙션 보더 |
| `--color-border-tertiary` | `#ECE8DF` | 카드/구분선 보더 |
| `--color-background-primary` | `#FFFFFF` | 카드/패널 표면 |
| `--color-background-secondary` | `#F7F4ED` | hover, pill, muted 영역 |
| `--color-background-tertiary` | `#F1EFE8` | 앱 배경 |

#### 타이포그래피

| 토큰 | 값 | weight | 용도 |
|------|----|--------|------|
| `text-display` | `28px / 1.75rem` | `700` | 페이지 타이틀 |
| `text-title` | `22px / 1.375rem` | `600` | 카드 제목, 섹션 헤더 |
| `text-body` | `16px / 1rem` | `400` | 일반 본문 |
| `text-caption` | `13px / 0.8125rem` | `400` | 보조 텍스트 |
| `text-micro` | `11px / 0.6875rem` | `500` | 뱃지, 메타 라벨 |

#### spacing / radius

| 토큰 | 값 | 적용 |
|------|----|------|
| `spacing-xs` | `4px` | 아이콘 내부 여백 |
| `spacing-sm` | `8px` | 컴포넌트 gap |
| `spacing-md` | `16px` | 기본 카드 패딩 |
| `spacing-lg` | `24px` | 섹션 간격 |
| `spacing-xl` | `40px` | 페이지 상단 여백 |
| `radius-sm` | `6px` | 태그, 뱃지 |
| `radius-md` | `10px` | 버튼, 입력 필드 |
| `radius-lg` | `14px` | 카드, 모달 |
| `radius-xl` | `20px` | 토스트, 강조 패널 |

---

### 폴더 구조 (Structure)

```text
src/
  app/                    # 앱 진입점, 전역 provider, 라우터 설정
  features/               # 도메인별 기능 모듈
    dashboard/            # Phase 1
    curriculum/           # Phase 2
    schedule/             # Phase 2
    habit/                # Phase 3
    gamification/         # Phase 3
    community/            # Phase 4
  shared/
    components/           # 공통 UI 컴포넌트
    hooks/                # 공통 훅
    stores/               # Zustand 전역 상태
    api/                  # API 클라이언트, endpoint wrapper
    utils/                # 포맷터, 순수 유틸
  styles/                 # 글로벌 CSS, tokens
```

#### 설계 원칙
- [ ] 기능별 UI와 로직은 `features/` 하위에 둔다.
- [ ] 여러 Phase에서 재사용되는 UI는 `shared/components/`에 둔다.
- [ ] 서버 통신 훅은 React Query 기반으로 `shared/api/` 또는 기능 모듈 내부에서 관리한다.
- [ ] 전역 브라우저 상태만 Zustand store로 승격한다.

---

### 라우터 구조 (Routes)

| 경로 | 컴포넌트 | 설명 | Phase | 인증 |
|------|----------|------|-------|------|
| `/` | `LandingPage` | 공개 진입/소개 또는 로그인 분기 | `phase-0` | 불필요 |
| `/login` | `LoginPage` | 인증 진입 화면 | `phase-0` | 불필요 |
| `/dashboard` | `DashboardPage` | 로그인 후 기본 대시보드 | `phase-1` | 필요 |
| `/curriculum` | `CurriculumPage` | 커리큘럼 목록/필터 | `phase-2` | 필요 |
| `/curriculum/:id` | `CurriculumDetailPage` | 교재 상세, 로드맵 뷰 | `phase-2` | 필요 |
| `/schedule` | `SchedulePage` | 주간/월간 캘린더 | `phase-2` | 필요 |
| `/habit` | `HabitPage` | 습관 관리, 학습 기록 | `phase-3` | 필요 |
| `/analytics` | `AnalyticsPage` | 학습 분석 차트 | `phase-3` | 필요 |
| `/community` | `CommunityPage` | 커뮤니티 피드 | `phase-4` | 필요 |
| `/settings` | `SettingsPage` | 계정/알림 설정 | `phase-0` | 필요 |

#### 인증 라우팅 정책

- [ ] 앱 시작 시 인증 상태가 확정되기 전까지 보호 라우트는 즉시 리다이렉트하지 않는다.
- [ ] 인증 부트스트랩 중에는 앱 셸 로더 또는 splash 상태를 보여준다.
- [ ] 인증되지 않은 사용자가 보호 라우트에 접근하면 `/login`으로 이동한다.
- [ ] 인증된 사용자가 `/login`에 접근하면 `/dashboard`로 리다이렉트한다.

---

### 목업 반영 기준 (Mockup Alignment)

> `self_directed_learning_dashboard.html` 목업을 기준으로, Phase 1 대시보드는 문서상의 기능 요구뿐 아니라 실제 화면 정보구조와 밀도도 함께 반영한다.

#### 대시보드 정보 구조

- [ ] 전체 레이아웃은 `Sidebar(200px) + Main(flex)` 구조를 기본으로 한다.
- [ ] Sidebar는 `메인 / 성장 / 커뮤니티` 3개 섹션 그룹을 가진다.
- [ ] Sidebar 하단에는 현재 사용자 요약 카드(이름, 학년, 구독 상태)를 둔다.
- [ ] Topbar는 `페이지 제목 + 기간 탭(일간/주간/월간) + 알림 버튼` 순서로 배치한다.
- [ ] 본문은 `metrics-row -> row2 -> row3 -> notification-row` 순서를 유지한다.

#### 위젯 조합

- [ ] `metrics-row`는 4개의 핵심 지표 카드로 구성한다.
- [ ] `row2`는 `오늘의 학습 계획(2fr)`과 `과목별 월간 진도(1fr)`로 구성한다.
- [ ] `row3`는 `습관 & 게이미피케이션`, `학습 타이머`, `미니 캘린더` 3분할 구조를 사용한다.
- [ ] 하단 알림 센터는 3열 카드형 알림 목록을 기본 레이아웃으로 사용한다.

#### 시각 스타일

- [ ] 기본 카드 스타일은 흰색 표면 + 얇은 보더 + `radius-lg`를 사용한다.
- [ ] 배경은 단색 화이트가 아니라 약한 웜 톤의 `--color-bg` 계열을 사용한다.
- [ ] 과목별 색상은 진도바, 일정 좌측 컬러바, 상태 텍스트에 일관되게 재사용한다.
- [ ] 활성 네비게이션은 `primary-light` 배경과 `primary-dark` 텍스트 조합을 사용한다.
- [ ] 밀도는 “모바일 앱보다 약간 여유 있고, B2B 대시보드보다는 부드러운” 중간 밀도를 유지한다.

#### 반응형 기준

- [ ] `lg (>=1280px)`에서는 목업의 4열/2열/3열 레이아웃을 유지한다.
- [ ] `md (768~1279px)`에서는 Sidebar를 아이콘 중심 축약형으로 바꾸고, 본문은 2열 위주로 재배치한다.
- [ ] `sm (<768px)`에서는 Sidebar를 하단 탭바 또는 드로어로 전환하고 모든 위젯을 1열 스택으로 배치한다.

---

### 공통 컴포넌트 기준선 (Shared Components)

| 컴포넌트 | 주요 Props | Phase | 우선순위 | 설명 |
|----------|------------|-------|----------|------|
| `Button` | `variant`, `size`, `loading`, `icon` | `phase-0` | `P0` | primary/outline/ghost 버튼 |
| `Card` | `padding`, `hover`, `className` | `phase-0` | `P0` | 기본 카드 래퍼 |
| `Badge` | `color`, `size`, `dot` | `phase-0` | `P0` | 상태 라벨 |
| `Avatar` | `name`, `src`, `size` | `phase-0` | `P0` | 이니셜 자동 생성 |
| `MetricCard` | `label`, `value`, `trend`, `trendLabel` | `phase-0` | `P0` | 대시보드 지표 카드 기반 |
| `ProgressBar` | `value`, `max`, `color`, `size` | `phase-0` | `P0` | 애니메이션 가능한 진행바 |
| `Modal` | `open`, `onClose`, `title` | `phase-0` | `P0` | 포커스 트랩 포함 |
| `Toast` | `type`, `message`, `duration` | `phase-0` | `P0` | Zustand 토스트 store 연동 |
| `EmptyState` | `icon`, `title`, `description`, `action` | `phase-0` | `P0` | 빈 상태 컴포넌트 |
| `Skeleton` | `width`, `height`, `variant` | `phase-0` | `P0` | 로딩 플레이스홀더 |

#### 목업 기반 추가 컴포넌트 후보

| 컴포넌트 | 역할 | 비고 |
|----------|------|------|
| `SidebarNav` | 섹션 라벨 + 네비게이션 아이템 그룹 | active 상태, badge 포함 |
| `TopbarTabs` | 일간/주간/월간 pill 탭 | 대시보드 period 상태 연동 |
| `PlanListItem` | 색상 바 + 체크 + 상세 정보 + 시간 표시 | 낙관적 업데이트 대응 |
| `SubjectProgressItem` | 과목명 + 퍼센트 + 진행바 | 색상 매핑 필요 |
| `HabitStreakItem` | 아이콘 + streak + 도트 배지 | 게이미피케이션 미리보기 |
| `TimerRing` | SVG 진행률 원형 타이머 | `timerStore` 연동 |
| `MiniCalendarGrid` | 날짜 셀 + 이벤트 점 | 오늘/이벤트 상태 표시 |
| `NotificationPreviewCard` | 아이콘 + 제목 + 시간 정보 | 알림 센터 3열 카드 |

---

### 상태 관리 (State Management)

| 스토어/훅 | 책임 | 기술 | API 연동 |
|-----------|------|------|----------|
| `authStore` | 인증 정보, 사용자 세션 | Zustand | JWT/refresh |
| `uiStore` | 토스트, 모달, 전역 UI 상태 | Zustand | 없음 |
| `dashboardStore` | 대시보드 기간 탭, 선택 날짜 | Zustand | 없음 |
| `timerStore` | 타이머 상태, 남은 시간, 세션 수 | Zustand | 세션 완료 시 연동 |
| `sessionStore` | 학습 기록 입력 상태 | Zustand | 세션 저장 API |
| `useMetrics()` | 지표 데이터 로드 | React Query | `GET /api/v1/metrics` |
| `useTodayPlan()` | 오늘의 계획 조회 | React Query | `GET /api/v1/plans/today` |
| `useNotifications()` | 알림 조회 | React Query + SSE | `GET /api/v1/notifications` |

#### 상태 분리 원칙
- [ ] 서버에서 진실 원본을 가지는 데이터는 React Query로 관리한다.
- [ ] 타이머, 모달, 토스트, 선택 탭 같은 클라이언트 UI 상태만 Zustand에 둔다.
- [ ] 낙관적 업데이트가 필요한 일정/좋아요/체크 토글은 `useMutation + onMutate`로 구현한다.
- [ ] 인증은 `bootstrap -> authenticated/unauthenticated` 흐름을 먼저 확정한다.

---

### API 연동 규약 (API Contracts)

#### 전역 규약

| 항목 | 규약 |
|------|------|
| Base URL | `/api/v1` |
| 인증 | `Authorization: Bearer <JWT>` |
| Refresh | `httpOnly` refresh cookie 권장, access token은 메모리 우선 |
| Content-Type | `application/json` |
| 파일 업로드 | `multipart/form-data` |
| 날짜 포맷 | ISO 8601 UTC |
| 페이지네이션 | `{ data, meta: { page, total } }` |
| 실시간 알림 | Phase 4 이전까지 SSE |

#### 에러 포맷

```typescript
interface ApiErrorResponse {
  code: string
  message: string
  details?: unknown
}
```

#### 핵심 엔드포인트

| 메서드 | 엔드포인트 | 기능 | 캐시 전략 |
|--------|------------|------|-----------|
| `GET` | `/api/v1/metrics` | 대시보드 지표 | period별 캐시 |
| `GET` | `/api/v1/plans/today` | 오늘 학습 계획 | 짧은 staleTime |
| `GET` | `/api/v1/notifications` | 알림 목록 | SSE 보강 |
| `POST` | `/api/v1/sessions` | 세션 완료 저장 | 성공 후 invalidate |
| `GET` | `/api/v1/curriculum/roadmap` | 학년별 로드맵 | `staleTime: 1d` |
| `GET` | `/api/v1/books` | 교재 목록/필터 | `staleTime: 1h` |
| `POST` | `/api/v1/recommend` | AI 추천 요청 | 캐시 없음 |
| `GET` | `/api/v1/schedule` | 일정 조회 | `staleTime: 5m` |
| `POST` | `/api/v1/schedule` | 일정 생성 | 생성 후 invalidate |
| `PATCH` | `/api/v1/schedule/:id` | 일정 수정/이동 | optimistic |
| `DELETE` | `/api/v1/schedule/:id` | 일정 삭제 | optimistic |

---

### Phase 계획 (Roadmap)

#### Phase 0 — 프로젝트 기반 설계
- 목표: 개발 환경 구성, 디자인 시스템 정의, 공통 컴포넌트 라이브러리 구축
- 기간: 1~2주
- 산출물:
  - 디자인 토큰
  - 공통 컴포넌트
  - 라우터 구조
  - 상태 관리 설계
- 완료 기준:
  - [ ] `styles/tokens.css`에 CSS 변수 정의
  - [ ] raw token + semantic token 레이어 정의
  - [ ] Storybook에 공통 컴포넌트 10종 문서화
  - [ ] React Router 레이아웃 셸 동작 확인
  - [ ] 공개/보호 라우트 및 auth bootstrap 흐름 확인
  - [ ] `authStore`, `uiStore` 초기 구조 설계
  - [ ] Vitest 환경 구성 및 `Button` 테스트 통과
  - [ ] Tailwind 커스텀 테마 설정 완료

#### Phase 1 — 대시보드 핵심 UI
- 목표: 지표, 계획, 진도, 타이머, 캘린더, 알림 대시보드 구현
- 기간: 2~3주
- 산출물:
  - `DashboardPage`
  - 7개 위젯 컴포넌트
  - 타이머 로직
  - 캘린더 뷰
- 레이아웃 기준:
  - Sidebar 200px 고정
  - Topbar 높이 56px 내외
  - Metrics 4열, Plan/Progress 2:1, Bottom 3열, Notifications 3열
- 완료 기준:
  - [ ] 위젯 렌더링 정상 동작
  - [ ] 일/주/월 탭 전환과 애니메이션 반영
  - [ ] 타이머 시작/일시정지/리셋/완료 토스트
  - [ ] PlanItem 체크 낙관적 업데이트
  - [ ] 3개 브레이크포인트 레이아웃 검증

#### Phase 2 — 커리큘럼 & 스케줄 관리
- 목표: 로드맵, 교재 DB, AI 추천, 스케줄 캘린더 구현
- 기간: 3주
- 산출물:
  - `CurriculumPage`
  - `RoadmapView`
  - `SchedulePage`
  - `CalendarView`
  - `AutoPlanModal`
- 완료 기준:
  - [ ] 학년 선택 기반 로드맵 필터링
  - [ ] 교재 검색/필터 + 300ms 디바운스
  - [ ] AI 추천 마법사 3단계 완료
  - [ ] 주간/월간 캘린더 뷰 전환
  - [ ] 드래그 앤 드롭 일정 이동
  - [ ] 자동 학습 계획 생성 후 캘린더 반영

#### Phase 3 — 습관 관리 & 게이미피케이션
- 목표: 학습 기록, 차트, 레벨/배지, 습관 목표 구현
- 기간: 2~3주
- 산출물:
  - `HabitPage`
  - `AnalyticsPage`
  - `GamificationPanel`
  - 습관 코칭 UI
- 완료 기준:
  - [ ] 학습 세션 기록 즉시 포인트 반영
  - [ ] 히트맵, 바차트, 시간대 차트 렌더링
  - [ ] 레벨업 오버레이 동작
  - [ ] 배지 Toast 동작
  - [ ] 습관 목표 CRUD
  - [ ] 포인트 히스토리 표시

#### Phase 4 — 커뮤니티 & 수익화
- 목표: 커뮤니티, Q&A, 프리미엄 결제, 광고/제휴 기능 구현
- 기간: 3주
- 산출물:
  - `CommunityPage`
  - `QAPage`
  - `PricingPage`
  - `PremiumGate`
- 완료 기준:
  - [ ] 커뮤니티 피드 무한 스크롤
  - [ ] 게시글 CRUD + 이미지 업로드
  - [ ] PremiumGate blur 오버레이
  - [ ] 결제 연동
  - [ ] 제휴 링크 클릭 추적
  - [ ] 광고 슬롯 lazy load 및 프리미엄 우회

---

### 핵심 UX 규칙 (Behavior)

| 상황 | 동작 |
|------|------|
| 사용자가 사이드바 메뉴를 선택 | 해당 섹션이 active 스타일로 표시되고 페이지 컨텍스트가 명확히 유지된다 |
| 대시보드 기간 탭 변경 | 지표와 애니메이션이 새 기간 기준으로 갱신된다 |
| 학습 계획 체크 완료 | 체크박스 상태와 카드 UI가 즉시 반영되고 서버와 동기화된다 |
| 타이머 0초 도달 | `completed` 상태로 전환되고 세션 저장 콜백이 호출된다 |
| 데이터 로딩 중 | 위젯 단위 Skeleton을 표시하고 전체 화면 블로킹은 피한다 |
| 위젯 데이터 에러 | 위젯별 재시도 버튼과 오류 상태를 독립적으로 표시한다 |
| 빈 데이터 | `EmptyState`로 안내하고 다음 액션 CTA를 제공한다 |
| 프리미엄 전용 기능 접근 | blur 미리보기 + 업그레이드 CTA를 표시한다 |
| 광고 슬롯 데이터 없음 | 자체 프로모션 카드로 폴백한다 |

---

### 접근성 및 품질 기준

- [ ] 키보드 내비게이션과 `focus-visible` 스타일을 지원한다.
- [ ] 텍스트와 UI 요소 색상 대비는 4.5:1 이상을 목표로 한다.
- [ ] 이미지에는 `alt`를 제공한다.
- [ ] 폼은 `label` 또는 `aria-labelledby`를 갖는다.
- [ ] 에러 메시지는 입력 필드와 `aria-describedby`로 연결한다.
- [ ] 토스트, 알림, 실시간 상태는 적절한 live region을 사용한다.
- [ ] 세션 만료 60초 전 경고를 제공한다.
- [ ] 모션은 `prefers-reduced-motion` 환경에서 축소한다.

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 각 위젯은 독립적으로 로딩/에러/빈 상태를 가질 수 있어야 한다.
- [ ] 타이머 상태는 새로고침 또는 라우트 이동 시 보존 전략을 정해야 한다.
- [ ] 일정 드래그 앤 드롭 중 서버 실패 시 롤백이 필요하다.
- [ ] AI 추천 응답 JSON 파싱 실패 시 대체 오류 경험이 필요하다.
- [ ] 대시보드 카드 수가 늘어날 때도 목업의 시각적 밀도가 과하게 깨지지 않아야 한다.
- [ ] 긴 과목명 또는 교재명이 `PlanListItem`과 알림 카드에서 레이아웃을 무너뜨리지 않아야 한다.
- [ ] 모바일에서 3열 알림 카드가 1열 또는 2열로 자연스럽게 재배치되어야 한다.
- [ ] 탭 전환 시 수치만 바뀌고 카드 높이가 흔들리지 않도록 고정 높이 전략이 필요하다.
- [ ] 프리미엄 사용자에게는 광고를 보여주지 않아야 한다.
- [ ] 커뮤니티 이미지 업로드는 최대 5장 제한을 적용해야 한다.
- [ ] 실시간 알림은 Phase 4까지 SSE를 기준으로 구현하고, 이후 WebSocket 전환 여지를 남긴다.
- [ ] UTC 기반 날짜 데이터는 클라이언트에서 로컬 타임존으로 안전하게 변환해야 한다.

---

### 세부 Spec 분리 대상

- [ ] `Clinet/components/Button.md`
- [ ] `Clinet/components/MetricCard.md`
- [ ] `Clinet/components/ProgressBar.md`
- [ ] `Clinet/components/Modal.md`
- [ ] `Clinet/components/SidebarNav.md`
- [ ] `Clinet/components/TopbarTabs.md`
- [ ] `Clinet/components/TimerRing.md`
- [ ] `Clinet/components/MiniCalendarGrid.md`
- [ ] `Clinet/features/dashboard-core.md`
- [ ] `Clinet/features/curriculum-management.md`
- [ ] `Clinet/features/schedule-management.md`
- [ ] `Clinet/features/habit-and-gamification.md`
- [ ] `Clinet/features/community-and-monetization.md`
- [ ] `Common/api/metrics-get.md`
- [ ] `Common/api/plans-today-get.md`
- [ ] `Common/api/sessions-create.md`
- [ ] `Common/api/curriculum-roadmap-get.md`
- [ ] `Common/api/books-get.md`
- [ ] `Common/api/recommend-create.md`
- [ ] `Common/api/schedule-list.md`
- [ ] `Clinet/store/auth-store.md`
- [ ] `Clinet/store/ui-store.md`
- [ ] `Clinet/store/timer-store.md`
- [ ] `Clinet/store/dashboard-store.md`

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 공통 컴포넌트 10종이 Storybook과 Vitest 기준으로 문서화 및 기본 렌더링 테스트를 가진다.
- [ ] `/dashboard`에서 7개 위젯이 개별 로딩 상태와 완료 상태를 올바르게 표시한다.
- [ ] Sidebar active 상태와 period 탭 active 상태가 시각적으로 정확히 반영된다.
- [ ] 타이머가 `idle -> running -> paused -> completed` 상태 전이를 올바르게 수행한다.
- [ ] 교재 검색 필터가 300ms 디바운스로 동작한다.
- [ ] `PlanListItem`의 긴 텍스트, 완료 상태, 진행 중 상태가 모두 안정적으로 렌더링된다.
- [ ] 미니 캘린더가 `today`, `has-event`, 빈 셀 상태를 정확히 구분해 표시한다.
- [ ] 일정 수정/삭제가 낙관적 업데이트 후 실패 시 롤백된다.
- [ ] 프리미엄 전용 기능 접근 시 `PremiumGate`가 노출된다.
- [ ] 접근성 체크리스트 항목이 핵심 페이지에서 충족된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 초기 프로젝트 아키텍처 Draft 작성 | Codex |
| 2026-03-28 | `StudyPath_프론트엔드_개발_스펙_v1.0` 기준으로 Phase 구조, 기술 스택, 디자인 토큰, 라우터, API 규약, 접근성 기준을 반영해 상세화 | Codex |
| 2026-03-28 | `self_directed_learning_dashboard.html` 목업 기준으로 대시보드 정보 구조, 레이아웃 비율, 추가 공통 컴포넌트 후보, 반응형/UX 제약을 반영 | Codex |

---

### 확인 (Approval)

- [ ] `StudyPath_프론트엔드_개발_스펙_v1.0` 기준 범위와 일치하는지 확인
- [ ] Phase 0부터 세부 Spec 분리 순서 확인
- [ ] 라우팅, 공통 컴포넌트, API 규약 기준 확정
