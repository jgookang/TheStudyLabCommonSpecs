## Spec: Community Post Detail Get

**유형**: `API`  
**위치**: `docs/specs/api/community-post-detail-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 커뮤니티 게시글 상세 화면에서 본문, 댓글, 관련 글을 함께 조회한다.

---

### 엔드포인트

- `GET /api/v1/community/posts/:postId`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### 응답

```json
{
  "data": {
    "post": {
      "id": "post-featured-1",
      "title": "고1 6월 모의고사 이후 수학 복습 루틴을 어떻게 가져가면 좋을까요?",
      "author": "수학집중반",
      "category": "질문",
      "excerpt": "기출 오답과 개념 복습을 함께 돌리는 주간 루틴을 짜고 싶은데, 우선순위를 어떻게 잡아야 할지 고민입니다.",
      "content": "모의고사 이후에는 틀린 문항만 다시 보는 방식보다...",
      "commentCount": 2,
      "likeCount": 42,
      "likedByMe": false,
      "timeLabel": "방금 전",
      "tags": ["수학", "모의고사", "복습루틴"]
    },
    "comments": [
      {
        "id": "comment-featured-1",
        "author": "루틴메이커",
        "content": "오답을 난도별로 다시 나누고, 개념 블록은 25분씩 짧게 끊어 두 번 반복하는 방식이 가장 안정적이었어요.",
        "timeLabel": "5분 전",
        "badgeLabel": "추천 답변"
      }
    ],
    "relatedPosts": [
      {
        "id": "post-1",
        "title": "영어 독해 루틴 4주 차 회고",
        "author": "리딩메이커",
        "category": "후기",
        "excerpt": "매일 30분 고정만으로도 체감 성과가 생겨서...",
        "commentCount": 1,
        "likeCount": 21,
        "likedByMe": false,
        "timeLabel": "12분 전"
      }
    ]
  }
}
```

---

### 클라이언트 메모

- query key: `['community', 'community-post-detail-get', postId]`
- remote service: `remoteCommunityService.getPostDetail(postId)`
- 게시글 상세에서 like / comment mutation 성공 시 같은 query key를 즉시 갱신한다.

---

### 에러

- `401 AUTH_REQUIRED`
- `404 COMMUNITY_POST_NOT_FOUND`
- `500 INVALID_COMMUNITY_POST_PAYLOAD`
