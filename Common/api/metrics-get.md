## Spec: GET /api/v1/dashboard/metrics

**?좏삎**: `API Endpoint`  
**?꾩튂**: `Common/api/metrics-get.md`  
**?묒꽦??*: 2026-03-28  
**?곹깭**: `Ready`

---

### 紐⑹쟻

> ??쒕낫???곷떒 吏??移대뱶???꾩슂???붿빟 ?섏튂瑜?議고쉶?쒕떎.  
> 湲곌컙(`daily`, `weekly`, `monthly`)???곕씪 ?숈뒿 ?쒓컙, 紐⑺몴 ?ъ꽦瑜? ?듦? ?먯닔, ?곗냽 ?숈뒿?쇱쓣 諛섑솚?쒕떎.

---

### ?붽뎄?ы빆

- [x] `period` query parameter 瑜?吏?먰븳??
- [x] ?뺥솗??4媛쒖쓽 ?듭떖 吏?쒕? 諛섑솚?쒕떎.
- [x] 媛?吏?쒕뒗 value, unit, trend ?뺣낫瑜??ы븿?쒕떎.
- [x] ?몄쬆???꾩슂?섎떎.

---

### ?명꽣?섏씠???뺤쓽

```typescript
type DashboardPeriod = 'daily' | 'weekly' | 'monthly'
type MetricTrend = 'up' | 'down' | 'neutral'

interface MetricsGetRequest {
  period: DashboardPeriod
}

interface MetricItem {
  id: 'study-time' | 'goal-rate' | 'habit-score' | 'streak-days'
  label: string
  value: string | number
  unit?: string
  trend: MetricTrend
  trendValue: string
  trendLabel: string
}

interface MetricsGetResponse {
  data: MetricItem[]
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
| `period` | ?꾩닔. `daily`, `weekly`, `monthly` 以??섎굹?ъ빞 ?쒕떎. |
| Authorization | ?꾩닔. ?좏슚??access token ?먮뒗 ?쒕쾭 ?몄쬆 而⑦뀓?ㅽ듃媛 ?덉뼱???쒕떎. |

寃利??ㅽ뙣 ??沅뚯옣 ?먮윭:
- ?섎せ??`period`: `400 INVALID_PERIOD`
- ?몄쬆 ?놁쓬: `401 AUTH_REQUIRED`

---

### ?숈옉 ?뺤쓽

| 議곌굔 | ?숈옉 |
|------|------|
| `period=daily` | ?ㅻ뒛 湲곗? 吏?쒕? 諛섑솚?쒕떎. |
| `period=weekly` | ?대쾲 二?湲곗? ?꾩쟻/鍮꾧탳 吏?쒕? 諛섑솚?쒕떎. |
| `period=monthly` | ?대쾲 ??湲곗? 吏?쒕? 諛섑솚?쒕떎. |
| ?곗씠???쇰? ?꾨씫 | ??ぉ???앸왂?섏? 留먭퀬 0 ?먮뒗 以묐┰ 媛믪쑝濡??뺢퇋?뷀븳?? |

---

### ?묐떟 洹쒖튃

- ?묐떟? ??긽 ?꾨옒 ?쒖꽌??4媛?metric ??諛섑솚?쒕떎.
  1. `study-time`
  2. `goal-rate`
  3. `habit-score`
  4. `streak-days`
- 鍮?諛곗뿴 ???zero-filled metric 諛곗뿴??諛섑솚?쒕떎.

---

### Query Key / Adapter

- React Query key: `['dashboard', 'metrics-get', period, mode]`
- remote DTO ??`data: MetricItem[]` 援ъ“瑜??ъ슜?섍퀬 ?붾㈃?먯꽌??洹몃?濡?metric 紐⑤뜽濡??ъ슜?쒕떎.

---

### ?뚯뒪???쒕굹由ъ삤

- [x] daily ?붿껌? ?뺤긽 ?묐떟?쒕떎.
- [x] weekly ?붿껌? ?뺤긽 ?묐떟?쒕떎.
- [x] monthly ?붿껌? ?뺤긽 ?묐떟?쒕떎.
- [ ] ?섎せ??`period` ?붿껌? `400 INVALID_PERIOD` 瑜?諛섑솚?쒕떎.
- [ ] ?몄쬆 ?녿뒗 ?붿껌? `401 AUTH_REQUIRED` 瑜?諛섑솚?쒕떎.
- [ ] ?곗씠?곌? 鍮꾩뼱??4媛?metric ??zero-filled ?뺥깭濡?諛섑솚?쒕떎.

---

### 蹂寃??대젰

| ?좎쭨 | 蹂寃??댁슜 | ?묒꽦??|
|------|-----------|--------|
| 2026-03-28 | metrics 議고쉶 API spec Ready 濡??뺤젙 | Codex |