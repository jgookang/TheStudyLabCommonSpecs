## Spec: Fetcher Transition Checklist

**?좏삎**: `API Endpoint`  
**?꾩튂**: `Clinet/api/fetcher-transition-checklist.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `In Progress`

---

### 紐⑹쟻

> mock 湲곕컲 ?곗씠???먮쫫???ㅼ젣 `/api/v1` fetcher濡??덉쟾?섍쾶 ?꾪솚?섍린 ?꾪븳 怨듯넻 泥댄겕由ъ뒪?몃떎.  
> 臾몄꽌, service, adapter, query, smoke test媛 媛숈? 怨꾩빟??蹂대룄濡?媛뺤젣?쒕떎.

---

### ?꾪솚 ?쒖꽌

1. endpoint spec?먯꽌 request, response, validation rule???뺤젙?쒕떎.
2. `src/shared/api/endpoints.ts` 寃쎈줈媛 spec ?대쫫怨??쇱튂?섎뒗吏 ?뺤씤?쒕떎.
3. remote service媛 ?ㅼ젣 DTO瑜?諛쏆븘?ㅻ룄濡?援ы쁽?쒕떎.
4. UI 紐⑤뜽怨?DTO媛 ?ㅻⅤ硫?adapter瑜??붾떎.
5. query key ?대쫫??spec 臾몄꽌紐낃낵 ?뺣젹?쒕떎.
6. remote service test濡?endpoint ?몄텧 怨꾩빟??怨좎젙?쒕떎.
7. adapter test濡?DTO -> UI 紐⑤뜽 蹂??洹쒖튃??怨좎젙?쒕떎.
8. feature蹂?`VITE_*_DATA_SOURCE=remote` ?꾪솚 ??smoke test瑜??ㅽ뻾?쒕떎.
9. ?ㅼ젣 ?묐떟??fixture? spec????컲?곹븳??

---

### 怨듯넻 泥댄겕由ъ뒪??
- [x] endpoint path ?곸닔媛 理쒖떊 spec怨??쇱튂?쒕떎.
- [x] request payload, query param, validation rule??臾몄꽌??紐낆떆???덈떎.
- [x] ?몄쬆 諛⑹떇怨?cookie/token 湲곗???臾몄꽌??紐낆떆???덈떎.
- [x] remote service test媛 endpoint ?몄텧 怨꾩빟??寃利앺븳??
- [x] adapter test媛 DTO -> UI 紐⑤뜽 蹂??洹쒖튃??寃利앺븳??
- [x] mock怨?remote媛 媛숈? 理쒖쥌 UI 紐⑤뜽??諛섑솚?쒕떎.
- [x] empty, error, unauthenticated 耳?댁뒪媛 ?뚯뒪?몃줈 蹂댄샇?쒕떎.
- [x] `.env.example`???꾩옱 吏?먰븯??data source ?ㅼ젙???뺣━???덈떎.
- [ ] ?ㅼ젣 `/api/v1` ?쒕쾭 ?묐떟?쇰줈 smoke test瑜??꾨즺?쒕떎.
- [ ] ?ㅼ쓳??湲곗??쇰줈 fixture? spec??媛깆떊?쒕떎.

---

### Feature蹂??곗꽑?쒖쐞

#### 1. Web Auth
- [ ] `POST /api/v1/auth/web/login`
- [ ] `POST /api/v1/auth/web/refresh`
- [ ] `POST /api/v1/auth/web/logout`
- [ ] same-site cookie / `401` refresh ?뺤콉 ?뺤씤
- [ ] timeout / error / session expired UX ?뺤씤

#### 2. Dashboard Read
- [ ] `GET /api/v1/dashboard/metrics`
- [ ] `GET /api/v1/dashboard/notifications`
- [ ] `GET /api/v1/dashboard/plans/today`
- [ ] `GET /api/v1/dashboard/habits`

#### 3. Feature Overview Read
- [ ] curriculum roadmap
- [ ] schedule board
- [ ] habit hub
- [ ] analytics overview / compare
- [ ] community feed / post detail

#### 4. Mutation
- [ ] today plans status
- [ ] habit check
- [ ] schedule create / update / delete
- [ ] community comment / like / report

---

### 紐⑤컮??硫붾え

- 紐⑤컮??auth transport ??`CommonSpecs\Common\api\auth-mobile-*` 臾몄꽌瑜?湲곗??쇰줈 愿由ы븳??
- ????μ냼 fetcher 泥댄겕由ъ뒪?몄쓽 吏곸젒 ??곸? web client endpoint ?대떎.

---

### ?섍꼍 蹂??洹쒖튃

?곗꽑?쒖쐞??`feature env > VITE_API_DATA_SOURCE > 湲곕낯媛?mock)` ?대떎.

| 蹂??| ?덉떆 | ??븷 |
|------|------|------|
| `VITE_API_BASE_URL` | `http://localhost:8080` | ?ㅼ젣 backend origin |
| `VITE_API_DATA_SOURCE` | `remote` | ?꾩껜 怨듯넻 湲곕낯媛?|
| `VITE_AUTH_DATA_SOURCE` | `remote` | auth留?媛뺤젣 remote |
| `VITE_DASHBOARD_DATA_SOURCE` | `remote` | dashboard留?媛뺤젣 remote |
| `VITE_CURRICULUM_DATA_SOURCE` | `remote` | curriculum留?媛뺤젣 remote |
| `VITE_SCHEDULE_DATA_SOURCE` | `remote` | schedule留?媛뺤젣 remote |
| `VITE_HABIT_DATA_SOURCE` | `remote` | habit留?媛뺤젣 remote |
| `VITE_ANALYTICS_DATA_SOURCE` | `remote` | analytics留?媛뺤젣 remote |
| `VITE_COMMUNITY_DATA_SOURCE` | `remote` | community留?媛뺤젣 remote |

---

### ?꾨즺 湲곗?

- [ ] web auth遺??二쇱슂 feature源뚯? remote 紐⑤뱶?먯꽌 濡쒓렇???먮쫫???좎??쒕떎.
- [ ] `npm.cmd run smoke:remote -- --base-url <backend-origin> --capture-fixtures` 媛 ?깃났?쒕떎.
- [ ] fixture? API spec???ㅼ젣 payload? ?쇱튂?쒕떎.
- [ ] mock ?쒓굅 吏곸쟾源뚯? regression test媛 怨꾩냽 ?듦낵?쒕떎.

---

### 愿??臾몄꽌

- [external-api-rollout-plan.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/external-api-rollout-plan.md)
- [remote-smoke-test-runbook.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-runbook.md)
- [backend-payload-capture.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/backend-payload-capture.md)