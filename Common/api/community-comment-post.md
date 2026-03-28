## Spec: Community Comment Post

**유형**: `API`  
**위치**: `Common/api/community-comment-post.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 특정 커뮤니티 게시글에 댓글을 등록하고, 갱신된 상세 데이터를 돌려준다.

---

### 엔드포인트

- `POST /api/v1/community/posts/:postId/comments`

---

### 요청

```json
{
  "content": "문제 풀이 루틴은 오답 복기와 개념 블록을 하루씩 나누는 방식이 좋았어요."
}
```

---

### 응답

- 응답 본문은 `community-post-detail-get`과 동일한 상세 payload를 반환한다.
- 서버는 새 댓글을 반영한 `comments`, `commentCount`를 함께 내려준다.

---

### 클라이언트 메모

- mutation hook: `useCreateCommunityComment(postId)`
- remote service: `remoteCommunityService.createComment(postId, content)`
- 성공 시 detail query와 feed query의 `commentCount`를 함께 갱신한다.

---

### 에러

- `400 INVALID_COMMUNITY_COMMENT`
- `401 AUTH_REQUIRED`
- `404 COMMUNITY_POST_NOT_FOUND`
