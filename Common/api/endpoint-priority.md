## Spec: Endpoint Priority

**?좏삎**: `API`  
**?꾩튂**: `Common/api/endpoint-priority.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Ready`

---

### 紐⑹쟻

> ?섎굹??諛깆뿏?쒓? ???꾨줎?몄? 紐⑤컮???깆쓣 紐⑤몢 吏?먰븷 ???덈룄濡? 
> ?대뼡 endpoint瑜??대뼡 ?쒖꽌濡?怨좎젙?좎? ?곗꽑?쒖쐞瑜??뺣━?쒕떎.

---

### P0. Web Launch Foundation

- [x] `POST /api/v1/auth/web/login`
- [x] `POST /api/v1/auth/web/refresh`
- [x] `POST /api/v1/auth/web/logout`
- [x] `GET /api/v1/dashboard/metrics`
- [x] `GET /api/v1/dashboard/plans/today`
- [x] `GET /api/v1/dashboard/notifications`

### P1. Shared Mobile Auth Foundation

- [x] `POST /api/v1/auth/mobile/login`
- [x] `POST /api/v1/auth/mobile/refresh`
- [x] `POST /api/v1/auth/mobile/logout`

### P2. Current Screen Endpoints

- [x] `GET /api/v1/dashboard/habits`
- [x] `GET /api/v1/curriculum/roadmaps`
- [x] `GET /api/v1/curriculum/roadmaps/:roadmapId`
- [x] `GET /api/v1/schedule/board?view=week|month`
- [x] `POST /api/v1/schedule`
- [x] `PATCH /api/v1/schedule/:scheduleId`
- [x] `DELETE /api/v1/schedule/:scheduleId`
- [x] `GET /api/v1/habits`
- [x] `POST /api/v1/habits/check`
- [x] `GET /api/v1/analytics/overview`
- [x] `GET /api/v1/analytics/compare`
- [x] `GET /api/v1/community/feed`
- [x] `GET /api/v1/community/posts/:postId`
- [x] `POST /api/v1/community/posts/:postId/comments`
- [x] `POST /api/v1/community/posts/:postId/like`
- [x] `POST /api/v1/community/posts/:postId/report`

### P3. Next Expansion

- [ ] premium / billing 愿??endpoint

---

### 硫붾え

- ??auth ??same-site + refresh always `401` 湲곗??쇰줈 怨좎젙?쒕떎.
- 紐⑤컮??auth ??媛숈? ?몄쬆 肄붿뼱瑜??곕릺 transport 留?遺꾨━?쒕떎.
- dashboard P0 spec ? 援ы쁽 ?꾩뿉 `Ready` ?곹깭濡?癒쇱? ?ル뒗??
- query key??endpoint spec ?대쫫怨?媛숈? 異뺤쑝濡?留욎텣??
- ?ㅼ젣 諛깆뿏??payload瑜?諛쏆쑝硫?fixture? spec??諛붾줈 媛깆떊?쒕떎.