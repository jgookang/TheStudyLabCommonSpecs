## Spec: External API Rollout Plan

**?좏삎**: `API Endpoint`  
**?꾩튂**: `Clinet/api/external-api-rollout-plan.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Ready`

---

### 紐⑹쟻

> StudyPath ???꾨줎?몃? mock 以묒떖 媛쒕컻 ?곹깭?먯꽌 ?ㅼ젣 backend ?곕룞 ?곹깭濡??먯쭊 ?꾪솚?쒕떎.  
> 由ъ뒪?щ? 以꾩씠湲??꾪빐 ??auth, 議고쉶, mutation???쒖감?곸쑝濡??곕떎.

---

### ?ㅺ퀎 ?먯튃

1. UI??洹몃?濡??먭퀬 data source留?諛붽씔??
2. mock怨?remote??媛숈? UI 紐⑤뜽??諛섑솚?댁빞 ?쒕떎.
3. ?ㅼ젣 ?묐떟???뺤씤?섍린 ?꾩뿉??adapter? fixture瑜?癒쇱? 怨좎젙?쒕떎.
4. feature ?꾩껜瑜???踰덉뿉 諛붽씀吏 留먭퀬 web auth -> read -> mutation ?쒖꽌濡??곕떎.
5. smoke test ?깃났 ?꾩뿉???쒖뿰???꾨즺?앸줈 ?먮떒?섏? ?딅뒗??

---

### 以鍮꾨Ъ

#### 諛깆뿏?쒖뿉??諛쏆븘?????뺣낫

- ?ㅼ젣 backend origin
- ?뚯뒪??怨꾩젙 1媛??댁긽
- auth 諛⑹떇
  - web auth transport: same-site `HttpOnly cookie`
  - web refresh ?ㅽ뙣 ????긽 `401`
  - mobile auth transport: `accessToken + refreshToken` rotation
- CORS / credentials / Origin ?덉슜 ?뺤콉
- ?쒖? ?먮윭 payload ?덉떆
  - `401`
  - `403`
  - `422`
  - `500`
- nullable / optional field 洹쒖튃
- ?섏씠吏?ㅼ씠?? ?꾪꽣, ?뺣젹 洹쒖튃

#### ?꾨줎?몄뿉??以鍮꾪븷 寃?
- `.env` ?먮뒗 ?ㅽ뻾 ?몄옄濡?`VITE_API_BASE_URL` ?ㅼ젙
- feature蹂?`VITE_*_DATA_SOURCE=remote` ?꾪솚 湲곗? ?뺤젙
- smoke test 怨꾩젙 ?섍꼍 蹂???낅젰
- fixture overwrite ?뺤콉 ?뺤씤

---

### ?④퀎蹂??ㅽ뻾 怨꾪쉷

#### Phase A. Web Auth 怨좎젙

紐⑺몴: ??濡쒓렇?멸낵 ?몄뀡 蹂듦뎄媛 ?ㅼ젣 backend?먯꽌 ?덉젙?곸쑝濡??ロ엳?붿? 癒쇱? ?뺤씤?쒕떎.

- [ ] `POST /api/v1/auth/web/login`
- [ ] `POST /api/v1/auth/web/refresh`
- [ ] `POST /api/v1/auth/web/logout`
- [ ] `AuthBootstrap` timeout, error, retry ?먮쫫 寃利?- [ ] 濡쒓렇?꾩썐 ??`/login` 蹂듦? ?뺤씤

?꾨즺 湲곗?:
- ?덈줈怨좎묠 ???몄뀡 蹂듦뎄媛 ?쒕떎.
- ?몄뀡 ?놁쓣 ??濡쒓렇???붾㈃???щ떎.
- 留뚮즺 ?몄뀡? `ready + isAuthenticated=false`濡??ロ엺??
- `401 REFRESH_SESSION_*` ????auth service ?먯꽌 鍮꾨줈洹몄씤 ?곹깭濡??뺢퇋?붾맂??

#### Phase B. 議고쉶???붾㈃ ?꾪솚

紐⑺몴: ?곌린 湲곕뒫蹂대떎 癒쇱? overview/read ?붾㈃???ㅼ젣 ?곗씠?곕줈 ?꾪솚?쒕떎.

- [ ] dashboard metrics / notifications / plans / habits
- [ ] curriculum roadmap / roadmap detail
- [ ] schedule board week / month
- [ ] habit hub
- [ ] analytics overview / compare
- [ ] community feed / post detail

?꾨즺 湲곗?:
- 媛?page媛 `remote`?먯꽌??mock怨?媛숈? ?붾㈃ 援ъ“瑜??좎??쒕떎.
- empty / error / partial response媛 adapter濡??덉쟾?섍쾶 泥섎━?쒕떎.

#### Phase C. Mutation ?꾪솚

紐⑺몴: ?ㅼ젣 ?ъ슜???≪뀡???쒕쾭 ?곹깭瑜?諛붽씀??吏?먮뱾???쒖감 ?꾪솚?쒕떎.

- [ ] dashboard today plans status
- [ ] habit check
- [ ] schedule create / update / delete
- [ ] community comment
- [ ] community like
- [ ] community report

?꾨즺 湲곗?:
- optimistic update? ?ㅼ젣 ?묐떟 寃곌낵媛 異⑸룎?섏? ?딅뒗??
- ?쒕쾭 validation error瑜?UI媛 蹂듦뎄 媛?ν븯寃?蹂댁뿬以??

#### Phase D. ?ㅼ쓳??諛섏쁺

紐⑺몴: 媛쒕컻??fixture? spec 臾몄꽌瑜??ㅼ꽌踰??묐떟 湲곗??쇰줈 留욎텣??

- [ ] capture fixture overwrite
- [ ] DTO ?꾨뱶 李⑥씠 adapter 諛섏쁺
- [ ] API spec request/response ?덉떆 媛깆떊
- [ ] remote smoke 寃곌낵 臾몄꽌 媛깆떊

?꾨즺 湲곗?:
- fixture, spec, remote service test媛 ?ㅼ젣 ?묐떟怨??쇱튂?쒕떎.

---

### 紐⑤컮??硫붾え

- 紐⑤컮??auth ??媛숈? 諛깆뿏?쒕? ?ъ슜?섏?留?transport 怨꾩빟? `CommonSpecs\Common\api\auth-mobile-*` 瑜?湲곗??쇰줈 蹂꾨룄 吏꾪뻾?쒕떎.
- ?꾩옱 ????μ냼??rollout blocker ??mobile ???꾨땲??web auth smoke ?듦낵??

---

### 沅뚯옣 ?꾪솚 ?쒖꽌

1. Web auth only remote
2. Dashboard only remote
3. Curriculum / Schedule / Habit / Analytics / Community read remote
4. Schedule CRUD remote
5. Habit mutation remote
6. Community mutation remote
7. ?꾩껜 feature remote

---

### ?ㅽ뻾 紐낅졊 ?덉떆

```powershell
npm.cmd run smoke:remote -- --base-url http://localhost:8080 --capture-fixtures
```

```env
VITE_API_BASE_URL=http://localhost:8080
VITE_AUTH_DATA_SOURCE=remote
VITE_DASHBOARD_DATA_SOURCE=remote
STUDYPATH_SMOKE_EMAIL=student@studypath.dev
STUDYPATH_SMOKE_PASSWORD=demo1234
```

---

### ?ㅽ뙣 ?????
#### Web Auth ?ㅽ뙣

- backend origin??留욌뒗吏 ?뺤씤
- `Set-Cookie`, `credentials: include`, `Origin` ?ㅻ뜑 ?뺤씤
- same-site 諛고룷 媛?뺤씠 源⑥졇 ?덉? ?딆?吏 ?뺤씤
- refresh endpoint媛 濡쒓렇??吏곹썑 ?몄뀡???쎈뒗吏 ?뺤씤
- `401 REFRESH_SESSION_*` 瑜???auth service 媛 鍮꾨줈洹몄씤 ?곹깭濡??뺢퇋?뷀븯?붿? ?뺤씤

#### 議고쉶 ?ㅽ뙣

- endpoint path? query param ?대쫫 鍮꾧탳
- adapter?먯꽌 nullable ?꾨뱶 ????꾨씫 ?щ? ?뺤씤
- feature env媛 mock?몄? remote?몄? ?ㅼ떆 ?뺤씤

#### Mutation ?ㅽ뙣

- request body shape? validation rule 鍮꾧탳
- optimistic update rollback ?щ? ?뺤씤
- success payload??理쒖떊 ?쒕쾭 ?곹깭媛 ?ㅻ뒗吏 ?뺤씤

---

### ?꾨즺 湲곗?

- [ ] web auth, dashboard, feature overview, mutation??紐⑤몢 remote smoke瑜??듦낵?쒕떎.
- [ ] fixture? spec???ㅼ쓳??湲곗??쇰줈 ?뺣━?쒕떎.
- [ ] settings diagnostics?먯꽌 紐⑤뱺 feature source媛 `remote`濡?蹂댁씤??
- [ ] mock???꾧퀬???듭떖 ?ъ슜???먮쫫???ы쁽?쒕떎.

---

### 愿??臾몄꽌

- [fetcher-transition-checklist.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/fetcher-transition-checklist.md)
- [remote-smoke-test-runbook.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-runbook.md)
- [remote-smoke-test-results.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-results.md)
- [backend-payload-capture.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/backend-payload-capture.md)