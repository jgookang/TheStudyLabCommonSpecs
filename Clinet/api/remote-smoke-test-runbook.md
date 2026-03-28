## Spec: Remote Smoke Test Runbook

**?좏삎**: `API Endpoint`  
**?꾩튂**: `Clinet/api/remote-smoke-test-runbook.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Ready`

---

### 紐⑹쟻

?ㅼ젣 諛깆뿏?쒓? ?곌껐???섍꼍?먯꽌 `web auth -> dashboard -> habit -> schedule -> logout` ?먮쫫??鍮좊Ⅴ寃?寃利앺븯怨?
?깃났???묐떟??fixture? API spec??諛붾줈 諛섏쁺?쒕떎.

---

### ?꾩옱 ?ㅽ뻾 諛⑹떇

?먭꺽 smoke test??CLI ?щ꼫濡??ㅽ뻾?쒕떎.

```powershell
npm.cmd run smoke:remote -- --base-url http://localhost:8080 --capture-fixtures
```

湲곕낯媛믪? ?꾨옒 ?섍꼍 蹂?섎줈??二쇱엯?????덈떎.

```env
VITE_API_BASE_URL=http://localhost:8080
STUDYPATH_SMOKE_EMAIL=student@studypath.dev
STUDYPATH_SMOKE_PASSWORD=demo1234
```

---

### ?ъ쟾 議곌굔

- `/api/v1`瑜??쒓났?섎뒗 ?ㅼ젣 諛깆뿏?쒓? ?ㅽ뻾 以묒씠?댁빞 ?쒕떎.
- 濡쒓렇??媛?ν븳 ?뚯뒪??怨꾩젙??以鍮꾨릺???덉뼱???쒕떎.
- ??auth same-site cookie ?뺤콉??媛쒕컻 ?섍꼍?먯꽌 ?덉슜?섏뼱???쒕떎.
- ?꾨줎???깆씠 蹂꾨룄 ?ы듃?????덈떎硫?`VITE_API_BASE_URL`濡?backend origin??紐낆떆?댁빞 ?쒕떎.

---

### ?먭? ???
- `POST /api/v1/auth/web/login`
- `POST /api/v1/auth/web/refresh`
- `GET /api/v1/dashboard/metrics`
- `GET /api/v1/dashboard/notifications`
- `GET /api/v1/dashboard/plans/today`
- `GET /api/v1/dashboard/habits`
- `GET /api/v1/habits`
- `POST /api/v1/habits/check`
- `GET /api/v1/schedule/board?view=week`
- `GET /api/v1/schedule/board?view=month`
- `POST /api/v1/schedule`
- `PATCH /api/v1/schedule/:scheduleId`
- `DELETE /api/v1/schedule/:scheduleId`
- `POST /api/v1/auth/web/logout`

---

### ?ㅽ뻾 ?쒖꽌

1. web 濡쒓렇??2. ?몄뀡 蹂듦뎄
3. ??쒕낫??議고쉶
4. habit 議고쉶/泥댄겕
5. schedule 二쇨컙/?붽컙 議고쉶
6. schedule ?앹꽦
7. schedule ?섏젙
8. schedule ??젣
9. 濡쒓렇?꾩썐

---

### 寃곌낵 諛섏쁺 洹쒖튃

- ?깃났 ?묐떟? `--capture-fixtures` ?듭뀡?쇰줈 湲곗〈 fixture JSON????뼱?대떎.
- ??nullable ?꾨뱶, validation rule, error code媛 蹂댁씠硫????API spec 臾몄꽌瑜?媛깆떊?쒕떎.
- ?ㅽ뙣???④퀎??[remote-smoke-test-results.md](/C:/Users/jgook/repo/CommonSpecs/Clinet/api/remote-smoke-test-results.md)???좎쭨? ?④퍡 湲곕줉?쒕떎.

---

### 二쇱쓽 ?ы빆

- ?꾨줎??dev server? backend媛 媛숈? ?ы듃???덈떎怨?媛?뺥븯吏 ?딅뒗??
- `http://localhost:4000`泥섎읆 ?꾨줎??HTML留??묐떟?섎뒗 ?쒕쾭??遺숈쑝硫?`/api/v1/*`??404濡??앸궇 ???덈떎.
- ??寃쎌슦 fixture瑜?媛깆떊?섏? 留먭퀬, results 臾몄꽌??backend 誘몄뿰寃??곹깭瑜?癒쇱? 湲곕줉?쒕떎.
- `POST /api/v1/auth/web/refresh` ??`401 REFRESH_SESSION_*` ??web auth service ?먯꽌 鍮꾨줈洹몄씤 ?곹깭濡??뺢퇋?뷀븳??