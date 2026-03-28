## Spec: Community Report Post

**유형**: `API`  
**위치**: `docs/specs/api/community-report-post.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 커뮤니티 게시글 신고를 moderation 큐에 등록하고, 접수 결과를 클라이언트에 반환한다.

---

### 엔드포인트

- `POST /api/v1/community/posts/:postId/report`

---

### 요청

```json
{
  "reason": "misinfo",
  "details": "과목명과 학습 범위 설명이 실제 커리큘럼 기준과 다르게 보입니다."
}
```

---

### 응답

```json
{
  "data": {
    "reportId": "report-123",
    "postId": "post-featured-1",
    "reason": "misinfo",
    "details": "과목명과 학습 범위 설명이 실제 커리큘럼 기준과 다르게 보입니다.",
    "reportedAt": "2026-03-28T09:00:00.000Z"
  }
}
```

---

### 클라이언트 메모

- mutation hook: `useReportCommunityPost(postId)`
- remote service: `remoteCommunityService.reportPost(postId, payload)`
- 성공 시 상세 페이지는 신고 모달을 닫고 접수 toast를 표시한다.

---

### 에러

- `400 INVALID_COMMUNITY_REPORT`
- `401 AUTH_REQUIRED`
- `404 COMMUNITY_POST_NOT_FOUND`
