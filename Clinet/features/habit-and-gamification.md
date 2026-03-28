## Spec: Habit And Gamification

**유형**: `Feature`  
**위치**: `Clinet/features/habit-and-gamification.md`  
**작성일**: 2026-03-28  
**상태**: `In Progress`

---

### 목적

> habit 체크, streak, XP, 배지 시스템을 통해
> 자기주도 학습 루틴의 지속성을 눈에 보이게 관리한다.

---

### 범위

- [x] `/habit`
- [x] habit hub overview
- [x] streak / XP / 배지 진행률
- [x] habit checklist mutation
- [x] level-up / badge toast flow
- [ ] learning analytics 연결

---

### 현재 구현 기준

- [x] `habit-hub-get` query hook
- [x] mock / remote habit service
- [x] overview metric, checklist, badge progress, insight 카드
- [x] 완료 체크 mutation과 optimistic update
- [x] local habit rules helper로 XP / badge / insight 계산 공유
- [x] level-up / badge toast 연동
- [x] dashboard habit summary와 habit hub 데이터 정합성 확보
- [ ] analytics 화면과 성장 데이터 공유 규칙 정리

---

### 정합성 규칙

- dashboard의 `습관 & 성장` 카드는 habit hub 체크리스트 snapshot에서 파생한다.
- `weeklyXp`, `level`, `badgeLabel`은 habit hub overview / badge 계산과 같은 규칙을 따라야 한다.
- habit checklist mutation이 성공하거나 롤백되면 dashboard habits query도 같은 규칙으로 동기화한다.
- dashboard 카드의 개별 habit progress는 checklist 항목별 목표 횟수와 streak 기반 추정치를 사용한다.

---

### 다음 구현

- [x] `habit-check-post` API spec 추가
- [x] local XP / badge 계산 규칙 정리
- [x] dashboard habit summary와 habit hub 정합성 반영
- [x] level-up / badge 획득 toast 정책 정의
- [ ] 실제 서버 규칙과 XP / badge 계산 정렬
- [ ] analytics 화면과 성장 데이터 공유 규칙 정의
