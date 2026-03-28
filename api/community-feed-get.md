## Spec: Community Feed Get

**유형**: `API`  
**위치**: `docs/specs/api/community-feed-get.md`  
**작성일**: 2026-03-28  
**상태**: `Ready`

---

### 목적

> 커뮤니티 overview 화면에서 metric, featured post, 최신 피드, premium CTA를 조회한다.

---

### 엔드포인트

- `GET /api/v1/community/feed`

---

### 인증

- 세션 기반 인증 필요
- 인증 실패 시 `401 AUTH_REQUIRED`

---

### 응답

```json
{
  "data": {
    "metrics": [
      {
        "id": "active-members",
        "label": "활성 학습자",
        "value": 128,
        "unit": "명",
        "trendLabel": "지난주 대비 +14명"
      }
    ],
    "featuredPost": {
      "id": "post-featured-1",
      "title": "고1 6월 모의고사 이후 수학 복습 루틴을 어떻게 가져가면 좋을까요?",
      "author": "수학집중반",
      "category": "질문",
      "excerpt": "기출 오답과 개념 복습을 함께 돌리는 주간 루틴을 짜고 싶은데, 우선순위를 어떻게 잡아야 할지 고민입니다.",
      "commentCount": 2,
      "likeCount": 42,
      "likedByMe": false,
      "timeLabel": "방금 전"
    },
    "feed": [
      {
        "id": "post-1",
        "title": "영어 독해 루틴 4주 차 회고",
        "author": "리딩메이커",
        "category": "후기",
        "excerpt": "매일 30분 고정만으로도 체감 성과가 생겨서, 이번 달에 바뀐 점을 정리해봤습니다.",
        "commentCount": 1,
        "likeCount": 21,
        "likedByMe": false,
        "timeLabel": "12분 전"
      }
    ],
    "premium": {
      "title": "스터디 챌린지와 프리미엄 코칭",
      "description": "질문 피드와 학습 인증을 프리미엄 코칭 흐름으로 확장할 수 있는 준비 영역입니다.",
      "benefits": ["주간 챌린지 참여", "질문 우선 답변", "코칭 아카이브 열람"]
    }
  }
}
```

---

### 클라이언트 메모

- query key: `['community', 'community-feed-get']`
- remote service: `remoteCommunityService.getFeed()`
- data source env: `VITE_COMMUNITY_DATA_SOURCE`
- 상세 페이지에서 comment / like mutation이 성공하면 feed cache의 `commentCount`, `likeCount`, `likedByMe`도 함께 갱신한다.

---

### 에러

- `401 AUTH_REQUIRED`
- `500 INVALID_COMMUNITY_FEED_PAYLOAD`
