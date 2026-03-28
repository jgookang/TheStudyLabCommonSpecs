## Spec: Billing And Subscription

**유형**: `Feature`  
**위치**: `Clinet/features/billing-subscription.md`  
**작성일**: 2026-03-28  
**상태**: `Implemented`

---

### 목적

> 커뮤니티 premium CTA와 설정 화면이 같은 구독 관리 페이지로 연결되도록 하여
> 실제 결제 API 전 단계에서도 일관된 billing 진입 흐름을 제공한다.

---

### 범위

- [x] `/settings/billing`
- [x] 현재 플랜 표시
- [x] 플랜 카드 비교
- [x] 커뮤니티 CTA 연결
- [x] 설정 화면 연결
- [x] premium gate overlay CTA 연결
- [ ] 실제 결제 / 플랜 변경 API

---

### 구현 기준

- `BillingPage`는 현재 세션의 `subscription` 값을 읽어 현재 플랜을 표시한다.
- `CommunityPage`와 `CommunityPostDetailPage`의 premium CTA는 `/settings/billing`으로 이동한다.
- `CommunityPage`와 `CommunityPostDetailPage`의 free 세션 premium gate CTA도 같은 billing 화면으로 이동한다.
- `SettingsPage`에서도 같은 billing 화면에 진입할 수 있다.
- 실제 결제 API가 아직 없으므로 플랜 변경 버튼은 안내용 toast로 동작한다.
