## Spec: Session Handoff

**유형**: `Feature`  
**위치**: `Clinet/features/session-handoff.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 현재 세션에서 완료한 작업과 다음 세션의 시작 지점을 한 문서로 정리한다.  
> 다음 작업자가 바로 이어서 구현, 검증, 백엔드 연동을 진행할 수 있도록 handoff 기준을 고정한다.

---

### 현재 상태 요약

StudyPath 프론트는 이제 주요 라우트와 핵심 기능 흐름이 모두 구현된 상태다.

- [x] 공통 UI 컴포넌트와 앱 셸 구현
- [x] 인증 진입, 보호 라우트, 로그인, 로그아웃 흐름 구현
- [x] dashboard / curriculum / schedule / habit / analytics / community overview 구현
- [x] schedule CRUD 구현
- [x] community detail / comment / like / report / premium gate 구현
- [x] analytics compare / filter / URL sync 구현
- [x] mock / remote / service / adapter / query 구조 정리
- [x] remote smoke runner 및 payload capture fixture 준비
- [x] auth bootstrap timeout / error / retry UX 구현
- [x] auth bootstrap 상태를 `ready` 중심으로 단순화

---

### 이번 세션 핵심 변경

#### 1. 인증 상태머신 정리

- `bootstrapStatus`를 `initial / loading / ready / timed_out / error`로 정리
- 인증 여부는 `isAuthenticated` 단일 기준으로 분리
- 느린 네트워크를 즉시 비로그인으로 처리하지 않도록 수정
- 보호 라우트와 로그인 화면에 retry CTA 추가

관련 파일:
- [authStore.ts](/C:/Users/jgook/repo/TheStudyLab/src/shared/stores/authStore.ts)
- [AuthBootstrap.tsx](/C:/Users/jgook/repo/TheStudyLab/src/app/AuthBootstrap.tsx)
- [AuthGate.tsx](/C:/Users/jgook/repo/TheStudyLab/src/features/auth/components/AuthGate.tsx)
- [LoginPage.tsx](/C:/Users/jgook/repo/TheStudyLab/src/features/auth/pages/LoginPage.tsx)

#### 2. 외부 API 전환 계획 문서화

- 실제 backend 연동을 위한 rollout plan 추가
- fetcher transition checklist 정리
- 다음 단계 문서를 실행 순서 기준으로 재작성

관련 파일:
- [external-api-rollout-plan.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/external-api-rollout-plan.md)
- [fetcher-transition-checklist.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/fetcher-transition-checklist.md)
- [phase-1-next-steps.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/features/phase-1-next-steps.md)

---

### 최신 검증 상태

마지막 검증 기준:

- [x] `npm.cmd test -- --run`
- [x] `npm.cmd run build`

검증 결과:

- Vitest: `166 passed`
- Production build: `passed`

---

### 최근 중요 커밋

| 커밋 | 내용 |
|------|------|
| `8d931db` | external API rollout plan 문서 추가 |
| `63f7013` | auth bootstrap status를 `ready` 중심으로 단순화 |
| `5f209f1` | auth bootstrap recovery states 추가 |
| `5331223` | remote smoke test runner 추가 |
| `d181f03` | 로그인 화면 즉시 노출 흐름 반영 |

---

### 현재 남은 가장 큰 일

#### 1. 실제 백엔드 연결

가장 큰 blocker는 코드가 아니라 실제 backend 환경이다.

- [ ] 실제 backend origin 확보
- [ ] 테스트 계정 확보
- [ ] cookie / CORS / auth 정책 확인

#### 2. 실제 smoke test 실행

- [ ] auth smoke 성공
- [ ] dashboard / habit / schedule / analytics / community 조회 smoke 성공
- [ ] schedule CRUD / habit / community mutation smoke 성공

#### 3. 실응답 반영

- [ ] fixture overwrite
- [ ] spec response example 갱신
- [ ] adapter 필드 차이 반영
- [ ] results 문서 갱신

---

### 다음 세션 바로 할 일

#### Step 1. backend 연결 정보 반영

`.env` 또는 실행 인자로 아래 값을 준비한다.

```env
VITE_API_BASE_URL=http://localhost:8080
VITE_AUTH_DATA_SOURCE=remote
STUDYPATH_SMOKE_EMAIL=student@studypath.dev
STUDYPATH_SMOKE_PASSWORD=demo1234
```

#### Step 2. auth smoke부터 시작

```powershell
npm.cmd run smoke:remote -- --base-url <backend-origin> --capture-fixtures
```

먼저 확인할 항목:

- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `POST /api/v1/auth/web/logout`
- 로그인 후 새로고침 시 세션 유지 여부
- 세션 만료 시 `ready + isAuthenticated=false` 정리 여부

#### Step 3. read endpoint 순차 전환

우선순위:

1. dashboard
2. curriculum
3. schedule board
4. habit hub
5. analytics
6. community

#### Step 4. mutation 전환

우선순위:

1. schedule CRUD
2. habit check
3. community comment / like / report

---

### 참조 문서

- [project-foundation.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/features/project-foundation.md)
- [app-shell-auth.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/features/app-shell-auth.md)
- [phase-1-next-steps.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/features/phase-1-next-steps.md)
- [external-api-rollout-plan.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/external-api-rollout-plan.md)
- [fetcher-transition-checklist.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/fetcher-transition-checklist.md)
- [remote-smoke-test-runbook.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-runbook.md)
- [remote-smoke-test-results.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-results.md)

---

### handoff 메모

- 현재 워킹트리는 clean 상태를 유지하는 것이 좋다.
- 다음 세션의 첫 목표는 “새 기능 추가”가 아니라 “실백엔드 smoke 성공”이다.
- auth 상태머신은 정리됐으므로, 다음부터는 backend 응답 계약 확인에 집중하면 된다.
