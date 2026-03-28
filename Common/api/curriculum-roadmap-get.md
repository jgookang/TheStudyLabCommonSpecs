## Spec: GET /api/v1/curriculum/roadmaps

**유형**: `API Endpoint`  
**위치**: `Common/api/curriculum-roadmap-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 커리큘럼 목록 화면에서 사용자에게 노출할 로드맵 요약 목록을 조회한다.

---

### 요구사항

- 로드맵 목록을 배열로 반환한다.
- 각 항목은 `id`, `title`, `targetExam`, `gradeLabel`, `subjectCount`, `estimatedWeeks`, `completionRate`, `tagline`을 포함한다.
- 인증이 필요하다.

---

### Query Key / Service

- React Query key: `['curriculum', 'curriculum-roadmap-get']`
- remote service: `remoteCurriculumService.getRoadmaps()`
- capture fixture: `src/features/curriculum/api/fixtures/curriculum-roadmap-get.response.json`

---

### 서버 검증 규칙

- 인증 없음: `401 AUTH_REQUIRED`
- 필수 필드 누락: `500 INVALID_CURRICULUM_ROADMAP_PAYLOAD`

---

### 테스트 시나리오

- [x] roadmap 목록 endpoint를 호출한다.
- [x] remote service가 response wrapper를 언랩한다.
- [x] curriculum page가 query 결과를 카드로 렌더링한다.
