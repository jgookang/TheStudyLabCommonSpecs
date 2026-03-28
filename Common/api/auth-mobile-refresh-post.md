## Spec: POST /api/v1/auth/mobile/refresh

**?좏삎**: `API Endpoint`  
**?꾩튂**: `Common/api/auth-mobile-refresh-post.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Ready`

---

### 紐⑹쟻

> ??secure storage ????λ맂 refresh token ?쇰줈  
> 紐⑤컮???몄뀡??蹂듦뎄?섍굅??access token ???щ컻湲됲븳??

---

### ?붽뎄?ы빆

- [x] request body 濡?`refreshToken` ??諛쏅뒗??
- [x] ?깃났 ???ъ슜???뺣낫? ??access/refresh token ?띿쓣 諛섑솚?쒕떎.
- [x] refresh token ???녾굅??臾댄슚?섎㈃ `401` ??諛섑솚?쒕떎.

---

### ?명꽣?섏씠???뺤쓽

```typescript
interface AuthMobileRefreshRequest {
  refreshToken: string
  deviceId?: string
}

interface AuthUser {
  id: string
  name: string
  grade?: string
  subscription?: 'free' | 'premium' | 'family' | 'school'
}

interface AuthMobileSessionResponse {
  user: AuthUser
  accessToken: string
  refreshToken: string
}

interface ApiErrorResponse {
  code: string
  message: string
  details?: unknown
}
```

---

### ?쒕쾭 寃利?洹쒖튃

| ?꾨뱶 | 洹쒖튃 |
|------|------|
| `refreshToken` | ?꾩닔. ?좏슚??refresh token ?댁뼱???쒕떎. |

寃利??ㅽ뙣 ??沅뚯옣 ?먮윭:
- refresh token ?놁쓬: `401 REFRESH_SESSION_NOT_FOUND`
- refresh token 留뚮즺 ?먮뒗 臾댄슚: `401 REFRESH_SESSION_INVALID`

---

### ?숈옉 ?뺤쓽

| 議곌굔 | ?숈옉 |
|------|------|
| ?좏슚??refresh token 議댁옱 | ???몄뀡 ?뺣낫瑜?諛섑솚?쒕떎. |
| ?몄뀡 ?놁쓬 | ??긽 `401` ?몄쬆 ?먮윭瑜?諛섑솚?쒕떎. |
| ??foreground 蹂듦? | 紐⑤컮??restore session ?먮쫫?먯꽌 ?ъ슜?쒕떎. |

---

### ?뚯뒪???쒕굹由ъ삤

- [x] refresh endpoint 濡?refresh token ??POST ?쒕떎.
- [x] ?깃났 ??access/refresh token ???뚯쟾?쒕떎.
- [ ] refresh token ???놁쑝硫?`401 REFRESH_SESSION_NOT_FOUND` 瑜?諛섑솚?쒕떎.
- [ ] refresh token ??臾댄슚?섎㈃ `401 REFRESH_SESSION_INVALID` 瑜?諛섑솚?쒕떎.

---

### 蹂寃??대젰

| ?좎쭨 | 蹂寃??댁슜 | ?묒꽦??|
|------|-----------|--------|
| 2026-03-28 | mobile auth refresh endpoint spec 異붽? | Codex |