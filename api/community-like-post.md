## Spec: Community Like Post

**유형**: `API`  
**위치**: `docs/specs/api/community-like-post.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 특정 커뮤니티 게시글의 좋아요 상태를 toggle하고, 갱신된 상세 데이터를 반환한다.

---

### 엔드포인트

- `POST /api/v1/community/posts/:postId/like`

---

### 요청

- body 없음

---

### 응답

- 응답 본문은 `community-post-detail-get`과 동일한 상세 payload를 반환한다.
- 서버는 갱신된 `likeCount`, `likedByMe`를 함께 내려준다.

---

### 클라이언트 메모

- mutation hook: `useToggleCommunityLike(postId)`
- remote service: `remoteCommunityService.toggleLike(postId)`
- UI는 optimistic update를 적용하고, 성공 응답으로 detail / feed cache를 다시 고정한다.

---

### 에러

- `401 AUTH_REQUIRED`
- `404 COMMUNITY_POST_NOT_FOUND`
