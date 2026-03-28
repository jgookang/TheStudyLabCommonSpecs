## Spec: Phase 1 Next Steps

**유형**: `Feature`  
**위치**: `Server/features/phase-1-next-steps.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> 현재 구현된 StudyPath 프론트를 실제 backend 전환 직전 상태로 정리하고,  
> 다음 구현 우선순위를 명확히 고정한다.

---

### 현재 완료 범위

- [x] Vite + React + TypeScript + Tailwind 기반 구성
- [x] Phase 0 공통 UI 구현
- [x] auth / dashboard / curriculum / schedule / habit / analytics / community overview 구현
- [x] AppShell, AuthGate, `/login`, `/settings` 구현
- [x] schedule CRUD 구현
- [x] habit mutation, reward toast, local rule helper 구현
- [x] community detail / comment / like / report / premium gate 구현
- [x] analytics compare / filter / URL sync 구현
- [x] data source resolver, remote service contract test, payload capture fixture 정리
- [x] auth bootstrap timeout / error / retry 흐름 정리

---

### 현재 남은 큰 일

#### 1. 실제 API 검증

- [ ] 실제 backend origin 확보
- [ ] 실제 계정으로 remote smoke test 실행
- [ ] 실응답을 fixture와 spec에 반영

#### 2. 서버 규칙 정렬

- [ ] habit XP / badge / streak 계산 규칙과 서버 규칙 정렬
- [ ] moderation backend 응답 규칙 반영
- [ ] analytics 상세 차트 payload 확장

#### 3. 전체 remote 전환

- [ ] auth remote 고정
- [ ] dashboard remote 고정
- [ ] feature overview remote 고정
- [ ] mutation remote 고정

---

### 다음 우선순위

#### Step 1. Web auth remote 검증

- [ ] `POST /api/v1/auth/web/login`
- [ ] `POST /api/v1/auth/web/refresh`
- [ ] `POST /api/v1/auth/web/logout`
- [ ] same-site cookie / CORS / credentials / Origin 정책 확인

#### Step 2. Read endpoint remote 검증

- [ ] dashboard
- [ ] curriculum
- [ ] schedule board
- [ ] habit hub
- [ ] analytics
- [ ] community

#### Step 3. Mutation remote 검증

- [ ] dashboard plans status
- [ ] habit check
- [ ] schedule create / update / delete
- [ ] community comment / like / report

#### Step 4. Fixture / spec 반영

- [ ] capture fixture overwrite
- [ ] adapter 필드 차이 반영
- [ ] API spec 예시 갱신
- [ ] smoke test results 문서 갱신

#### Step 5. Mobile auth transport 설계 반영

- [ ] `POST /api/v1/auth/mobile/login`
- [ ] `POST /api/v1/auth/mobile/refresh`
- [ ] `POST /api/v1/auth/mobile/logout`
- [ ] secure storage 기반 refresh rotation 계약 확인

---

### 실행 기준 문서

- [external-api-rollout-plan.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/external-api-rollout-plan.md)
- [fetcher-transition-checklist.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/fetcher-transition-checklist.md)
- [remote-smoke-test-runbook.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-runbook.md)
- [backend-payload-capture.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/backend-payload-capture.md)

---

### 권장 바로 다음 행동

1. 실제 backend origin과 테스트 계정을 확보한다.
2. auth만 `remote`로 켜고 smoke test를 통과시킨다.
3. dashboard read를 `remote`로 전환한다.
4. schedule CRUD와 habit mutation을 실제 응답 기준으로 맞춘다.

---

### 변경 이력

| 날짜 | 변경 내용 | 작성자 |
|------|-----------|--------|
| 2026-03-28 | Phase 1 다음 단계 초안 작성 | Codex |
| 2026-03-28 | 외부 API 전환 계획 기준으로 재정리 | Codex |
