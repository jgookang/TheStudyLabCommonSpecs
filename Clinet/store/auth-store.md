## Spec: authStore

**?좏삎**: `Store`  
**?꾩튂**: `Clinet/store/auth-store.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Implemented`

---

### 紐⑹쟻

> ?몄쬆 ?몄뀡, 遺?몄뒪?몃옪 ?곹깭, ?몄뀡 ?ъ떆???먮쫫????怨녹뿉??愿由ы븳??  
> 珥덇린 吏꾩엯, 蹂댄샇 ?쇱슦??媛?? 濡쒓렇???깃났, 濡쒓렇?꾩썐, refresh ?ㅽ뙣瑜?紐⑤몢 媛숈? ?곹깭 怨꾩빟?쇰줈 ?ル뒗??

---

### ?곹깭 ?뺤쓽

```ts
type BootstrapStatus =
  | 'initial'
  | 'loading'
  | 'ready'
  | 'timed_out'
  | 'error'

interface AuthUser {
  id: string
  name: string
  grade?: string
  subscription?: 'free' | 'premium' | 'family' | 'school'
}

interface AuthSessionPayload {
  user: AuthUser
  accessToken: string | null
}

interface AuthStoreState {
  user: AuthUser | null
  accessToken: string | null
  isAuthenticated: boolean
  isRefreshing: boolean
  bootstrapStatus: BootstrapStatus
  bootstrapRequestKey: number
}
```

---

### ?≪뀡 ?뺤쓽

```ts
setSession(payload: AuthSessionPayload): void
clearSession(): void
startBootstrap(): void
finishBootstrap(payload: { user: AuthUser | null; accessToken: string | null }): void
markBootstrapTimedOut(): void
failBootstrap(): void
retryBootstrap(): void
startRefresh(): void
finishRefresh(payload: AuthSessionPayload | null): void
```

---

### ?숈옉 洹쒖튃

| ?≪뀡 | 湲곕? 寃곌낵 |
|------|-----------|
| `setSession` | `user`, `accessToken`, `isAuthenticated=true`, `bootstrapStatus='ready'` |
| `clearSession` | ?몄뀡 ?뺣낫瑜??쒓굅?섍퀬 `bootstrapStatus='ready'` |
| `startBootstrap` | `bootstrapStatus='loading'` |
| `finishBootstrap(user 議댁옱)` | ?몄쬆 ?깃났 ?곹깭濡??꾩씠 |
| `finishBootstrap(user ?놁쓬)` | 鍮꾩씤利??곹깭濡??꾩씠 |
| `markBootstrapTimedOut` | ?몄뀡??鍮꾩슦怨?`bootstrapStatus='timed_out'` |
| `failBootstrap` | ?몄뀡??鍮꾩슦怨?`bootstrapStatus='error'` |
| `retryBootstrap` | `bootstrapStatus='initial'` 濡??섎룎由ш퀬 `bootstrapRequestKey` 利앷? |
| `startRefresh` | `isRefreshing=true` |
| `finishRefresh(payload 議댁옱)` | refresh 寃곌낵 ?몄뀡?쇰줈 媛깆떊 |
| `finishRefresh(null)` | ?몄뀡 留뚮즺 ?먮뒗 refresh `401` ?뺢퇋??寃곌낵濡?蹂닿퀬 鍮꾩씤利??곹깭濡??꾩씠 |

---

### ??remote auth ?곕룞 湲곗?

- ???대씪?댁뼵?몃뒗 `/api/v1/auth/web/*` endpoint 瑜??ъ슜?쒕떎.
- `restoreSession()` 怨?`refreshSession()` ? `401 REFRESH_SESSION_NOT_FOUND`, `401 REFRESH_SESSION_INVALID` 瑜?`null` 濡??뺢퇋?뷀븳??
- store ????`null` ???ㅽ뙣媛 ?꾨땲???뺤긽?곸씤 鍮꾨줈洹몄씤 ?곹깭濡?泥섎━?쒕떎.

---

### ?쇱슦???곕룞 洹쒖튃

- `/` 吏꾩엯 ??`RootRedirectPage`??`isAuthenticated`留?蹂닿퀬 `/dashboard` ?먮뒗 `/login`?쇰줈 ?대룞?쒕떎.
- 怨듦컻 ?쇱슦??`/login`)??`bootstrapStatus='initial' | 'loading'`?댁뼱??利됱떆 ?뚮뜑留곷맂??
- 蹂댄샇 ?쇱슦?몃뒗 `bootstrapStatus='initial' | 'loading'` ?숈븞 濡쒕뵫 ?붾㈃??蹂댁뿬以??
- 蹂댄샇 ?쇱슦?몃뒗 `bootstrapStatus='timed_out' | 'error'`?????ъ떆??CTA瑜?蹂댁뿬以??
- 蹂댄샇 ?쇱슦?몃뒗 `bootstrapStatus='ready'` ?닿퀬 `isAuthenticated=false` ????`/login`?쇰줈 由щ떎?대젆?명븳??

---

### ?뚯뒪??泥댄겕由ъ뒪??
- [x] 珥덇린 ?곹깭??`initial` ?댁뼱???쒕떎.
- [x] `setSession` ???몄쬆 ?곹깭濡??꾩씠?섏뼱???쒕떎.
- [x] `clearSession` ??鍮꾩씤利??곹깭濡??꾩씠?섏뼱???쒕떎.
- [x] `markBootstrapTimedOut` ??`timed_out` ?곹깭媛 ?섏뼱???쒕떎.
- [x] `retryBootstrap` ? `bootstrapRequestKey` 瑜?利앷??쒖폒???쒕떎.
- [x] `finishRefresh(null)` ? ?몄뀡???뺣━?댁빞 ?쒕떎.

---

### 蹂寃??대젰

| ?좎쭨 | 蹂寃??댁슜 | ?묒꽦??|
|------|-----------|--------|
| 2026-03-28 | auth store 珥덇린 spec ?묒꽦 | Codex |
| 2026-03-28 | `timed_out`, `error`, `retryBootstrap`, refresh null 泥섎━ 諛섏쁺 | Codex |
| 2026-03-28 | web auth split 諛?refresh 401 ?뺢퇋??湲곗? 諛섏쁺 | Codex |