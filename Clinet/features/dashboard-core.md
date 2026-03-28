## Spec: Dashboard Core

**타입**: `Feature`
**위치**: `Clinet/features/dashboard-core.md`
**작성일**: 2026-03-28
**상태**: `Draft`

---

### 목적 (Purpose)

> StudyPath의 핵심 진입 화면인 대시보드를 정의한다.
> 이 화면은 학습 현황, 오늘의 계획, 과목별 진도, 습관/게이미피케이션, 타이머, 캘린더, 알림을 한 화면에서 빠르게 파악하도록 돕는다.

---

### 요구사항 (Requirements)

#### 기능 요구사항
- [ ] 대시보드는 Sidebar, Topbar, 4개 지표 카드, 4개 하위 위젯 그룹으로 구성한다.
- [ ] 기간 탭(`일간`, `주간`, `월간`) 전환 시 지표 데이터와 일부 텍스트가 변경된다.
- [ ] 오늘의 학습 계획은 완료, 진행 중, 예정 상태를 구분해 표시한다.
- [ ] 학습 계획 항목은 체크 토글을 지원한다.
- [ ] 과목별 월간 진도는 색상 기반 진행바로 표시한다.
- [ ] 학습 타이머는 시작, 일시정지, 리셋, 완료 동작을 지원한다.
- [ ] 미니 캘린더는 오늘 날짜와 이벤트 날짜를 구분한다.
- [ ] 알림 센터는 최대 3개의 요약 카드를 기본으로 보여준다.

#### 비기능 요구사항
- [ ] 각 위젯은 독립적으로 로딩, 에러, 빈 상태를 가져야 한다.
- [ ] 지표 카드와 주요 위젯은 skeleton으로 로딩 상태를 제공한다.
- [ ] 대시보드는 `lg`, `md`, `sm` 브레이크포인트에 대응해야 한다.
- [ ] 핵심 상호작용은 200ms 이내 피드백을 제공해야 한다.

---

### 인터페이스 정의 (Interface)

```typescript
type DashboardPeriod = 'daily' | 'weekly' | 'monthly'

type PlanStatus = 'pending' | 'in_progress' | 'done'

interface DashboardMetric {
  id: 'study-time' | 'goal-rate' | 'habit-score' | 'streak-days'
  label: string
  value: string | number
  unit?: string
  trend?: 'up' | 'down' | 'neutral'
  trendValue?: string
  trendLabel?: string
}

interface PlanItem {
  id: string
  subject: string
  book: string
  detail: string
  estimatedMin: number
  status: PlanStatus
  color: string
}

interface SubjectProgress {
  id: string
  subject: string
  progress: number
  color: string
}

interface HabitPreview {
  id: string
  title: string
  currentStreak: number
  dotsCompleted: number
  dotsTotal: number
  icon?: string
  color: string
}

interface CalendarEvent {
  id: string
  date: string
  title: string
  type: 'exam' | 'assignment' | 'study' | 'reminder'
}

interface NotificationPreview {
  id: string
  title: string
  description?: string
  timeLabel: string
  icon?: string
  tone?: 'default' | 'success' | 'info' | 'warning'
}

interface DashboardPageData {
  period: DashboardPeriod
  metrics: DashboardMetric[]
  todayPlans: PlanItem[]
  subjectProgress: SubjectProgress[]
  habitPreview: HabitPreview[]
  timerSubject?: string
  calendarEvents: CalendarEvent[]
  notifications: NotificationPreview[]
}
```

---

### 레이아웃 정의 (Layout)

#### 데스크톱
- [ ] Sidebar: `200px`
- [ ] Main Topbar: 높이 `56px` 내외
- [ ] Metrics Row: 4열 그리드, `gap: 10px`
- [ ] Row 2: `2fr / 1fr`
- [ ] Row 3: 3열 동일 분할
- [ ] Notifications: 3열 카드 그리드

#### 태블릿
- [ ] Sidebar는 축약형으로 전환한다.
- [ ] Row 2와 Row 3는 2열 또는 1열+1열 재배치를 허용한다.
- [ ] Notifications는 2열 카드 그리드를 사용한다.

#### 모바일
- [ ] Sidebar는 하단 탭바 또는 드로어로 대체한다.
- [ ] 모든 위젯은 1열로 쌓는다.
- [ ] 상단 탭은 스크롤 없이 터치 가능한 크기를 유지한다.

---

### 동작 정의 (Behavior)

| 상황 | 동작 |
|------|------|
| 기간 탭 전환 | 지표 값, 비교 텍스트, 일부 요약 문구를 해당 기간 기준으로 갱신한다 |
| 계획 항목 체크 | 낙관적으로 완료 상태를 반영하고 실패 시 롤백한다 |
| 타이머 시작 | `idle` 또는 `paused` 상태에서 `running`으로 전환한다 |
| 타이머 일시정지 | 남은 시간을 유지한 채 `paused` 상태로 전환한다 |
| 타이머 완료 | 세션 완료 콜백과 토스트를 트리거한다 |
| 캘린더 날짜 선택 | 선택 상태를 바꾸고 해당 날짜 상세 진입의 기반 상태를 갱신한다 |
| 알림 모두 읽음 | 알림 카운트와 미읽음 표시를 갱신한다 |

---

### 데이터 연동 (Data Sources)

| 영역 | 기술 | 엔드포인트 |
|------|------|------------|
| 기간 상태 | Zustand | 없음 |
| 지표 데이터 | React Query | `GET /api/v1/metrics` |
| 오늘 계획 | React Query | `GET /api/v1/plans/today` |
| 알림 | React Query + SSE | `GET /api/v1/notifications` |
| 세션 완료 저장 | Mutation | `POST /api/v1/sessions` |

---

### 로딩 / 에러 / 빈 상태

- [ ] Metrics는 4개 카드 skeleton을 보여준다.
- [ ] 오늘의 학습 계획은 3개 row skeleton을 보여준다.
- [ ] 과목별 진도는 진행바 skeleton을 보여준다.
- [ ] 미니 캘린더는 날짜 그리드 skeleton 또는 placeholder를 보여준다.
- [ ] 알림은 카드 3개 skeleton을 보여준다.
- [ ] 데이터가 없을 때는 `EmptyState`와 적절한 CTA를 보여준다.
- [ ] 에러는 위젯 단위로 격리하고 `다시 시도` 액션을 제공한다.

---

### 엣지 케이스 & 제약 (Edge Cases)

- [ ] 학습 계획이 0개일 때도 레이아웃이 비지 않고 CTA가 자연스럽게 보여야 한다.
- [ ] 계획 항목 제목과 세부 설명이 길어도 줄바꿈 또는 말줄임으로 정리되어야 한다.
- [ ] 지표 값 단위(`점`, `일`, `%`)가 붙어도 카드 높이가 흔들리지 않아야 한다.
- [ ] 캘린더의 시작 요일, 빈 셀, 오늘 날짜, 이벤트 점 상태가 충돌하지 않아야 한다.
- [ ] 타이머가 백그라운드 전환 후 복귀해도 실제 남은 시간이 크게 어긋나지 않아야 한다.

---

### 테스트 시나리오 (Test Scenarios)

- [ ] 기간 탭 전환 시 metrics와 비교 문구가 갱신된다.
- [ ] 계획 항목 체크 시 즉시 완료 스타일이 반영된다.
- [ ] 타이머 시작 후 카운트다운이 진행된다.
- [ ] 타이머 리셋 시 기본값 `25:00`으로 복귀한다.
- [ ] 미니 캘린더가 오늘 날짜와 이벤트 날짜를 각각 표시한다.
- [ ] 알림이 3개 이하일 때 균형 있게 배치된다.
- [ ] 모바일에서 위젯이 1열 스택으로 재배치된다.

---

### 변경 이력 (Changelog)

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | 대시보드 핵심 UI Feature Spec 초안 작성 | Codex |

---

### 확인 (Approval)

- [ ] 위젯 조합 확인
- [ ] 데이터 연동 범위 확인
- [ ] 반응형 기준 확인
