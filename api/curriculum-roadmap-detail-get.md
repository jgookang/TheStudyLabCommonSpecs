## Spec: GET /api/v1/curriculum/roadmaps/:roadmapId

**유형**: `API Endpoint`  
**위치**: `docs/specs/api/curriculum-roadmap-detail-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 커리큘럼 상세 화면에서 선택한 로드맵의 개요, 추천 교재, 주간 포커스, 과목별 진행률을 조회한다.

---

### 요구사항

- `roadmapId`는 유효한 로드맵 id여야 한다.
- 응답은 summary 필드와 함께 `overview`, `recommendedBooks`, `weeklyFocus`, `subjects`를 포함한다.
- 인증이 필요하다.

---

### Query Key / Service

- React Query key: `['curriculum', 'curriculum-roadmap-detail-get', roadmapId]`
- remote service: `remoteCurriculumService.getRoadmapDetail(roadmapId)`
- capture fixture: `src/features/curriculum/api/fixtures/curriculum-roadmap-detail-get.response.json`

---

### 서버 검증 규칙

- 인증 없음: `401 AUTH_REQUIRED`
- 존재하지 않는 로드맵: `404 CURRICULUM_ROADMAP_NOT_FOUND`
- 필수 필드 누락: `500 INVALID_CURRICULUM_ROADMAP_DETAIL_PAYLOAD`
