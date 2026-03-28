## Spec: Curriculum Management

**유형**: `Feature`  
**위치**: `Clinet/features/curriculum-management.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> 학년, 목표, 과목 기준으로 학습 로드맵을 탐색하고  
> 추천 교재와 연결된 자기주도 학습 경로를 제공한다.

---

### 범위

- [x] `/curriculum`
- [x] `/curriculum/:id`
- [x] roadmap 목록 query hook
- [x] roadmap 상세 query hook
- [ ] 로드맵 필터
- [ ] 추천 교재 확장
- [ ] AI 추천 CTA

---

### 현재 기준선

- [x] AppShell 안에서 접근 가능한 전용 라우트를 확보했다.
- [x] 목록/상세 페이지가 mock query 기준으로 실제 데이터를 렌더링한다.
- [x] mock / remote service 와 endpoint 경로 기준을 추가했다.
- [ ] 실제 curriculum API payload 는 아직 백엔드 응답으로 교체 전이다.

---

### 다음 구현

- [ ] `curriculum-roadmap-get` 실제 응답 fixture 반영
- [ ] grade / exam / subject filter state 설계
- [ ] roadmap card 와 detail section 컴포넌트 세분화
- [ ] 추천 교재 카드와 시작하기 CTA 연결
