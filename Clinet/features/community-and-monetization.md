## Spec: Community And Monetization

**유형**: `Feature`  
**위치**: `Clinet/features/community-and-monetization.md`  
**작성일**: 2026-03-28  
**상태**: `Implemented`

---

### 목적

> 커뮤니티 피드, 게시글 상세, 댓글, 좋아요, 신고, 프리미엄 진입 흐름을 통해
> 학습 확장 경험과 수익화 진입 지점을 자연스럽게 연결한다.

---

### 범위

- [x] `/community`
- [x] community feed overview
- [x] featured post
- [x] premium CTA
- [x] 게시글 상세
- [x] comment mutation
- [x] like mutation
- [x] settings / billing 연결
- [x] premium gate overlay
- [x] moderation / 신고 진입
- [x] moderation / report mutation
- [ ] moderation backend

---

### 현재 구현 기준

- [x] `community-feed-get` query hook
- [x] `community-post-detail-get` query hook
- [x] mock / remote community service
- [x] feed metric, featured post, 최신 피드, premium CTA 카드
- [x] 게시글 상세 페이지
- [x] comment 등록 mutation
- [x] like toggle mutation
- [x] premium CTA와 `/settings/billing` 연결
- [x] free 세션용 premium gate overlay
- [x] premium 세션용 unlocked premium card
- [x] 게시글 신고 modal과 local moderation feedback
- [x] `POST /api/v1/community/posts/:postId/report` mutation 연결
- [ ] moderation backend 연동

---

### 다음 구현

- [x] `community-post-detail-get` spec 추가
- [x] `community-comment-post` spec 추가
- [x] `community-like-post` spec 추가
- [x] `community-report-post` spec 추가
- [x] premium gate CTA와 settings / billing 연결
- [x] premium gate overlay 구현
- [ ] moderation backend와 신고 상태 화면 연결
