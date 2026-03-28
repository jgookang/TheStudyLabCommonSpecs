## Spec: Settings Preferences

**유형**: `Feature`
**위치**: `Clinet/features/settings-preferences.md`
**작성일**: 2026-03-28
**상태**: `Implemented`

---

### 목적

> 설정 페이지가 단순 placeholder가 아니라
> 사용자 선호 저장과 API 전환 진단을 함께 담당하도록 만든다.

---

### 범위

#### 저장되는 항목

- [x] 시험 일정 알림 on/off
- [x] 학습 리마인더 on/off
- [x] 추천 업데이트 on/off
- [x] 일일 목표 시간 120 / 180 / 240 / 300분 선택
- [x] 집중 / 짧은 휴식 / 긴 휴식 시간 선택
- [x] 선호 과목 선택

#### API diagnostics

- [x] auth / dashboard / curriculum / schedule / habit / analytics / community data source 표시
- [x] 주요 `/api/v1` endpoint 경로 표시
- [x] remote smoke test 준비 상태 표시

#### 반영 범위

- [x] 대시보드 알림 섹션에 알림 설정 반영
- [x] 대시보드 목표 / 타이머에 학습 설정 반영
- [x] 선호 과목 변경 시 타이머 기본 문맥 반영
- [x] `/settings/billing` 구독 관리 화면 연결
- [x] 로그아웃 액션 연결

---

### 완료 기준

- [x] 새로고침 후에도 설정 값이 유지된다.
- [x] 설정 초기화 버튼으로 기본값 복구가 가능하다.
- [x] 설정 페이지에서 현재 feature별 data source와 endpoint를 확인할 수 있다.
- [x] remote 전환 준비 상태를 한 화면에서 판단할 수 있다.
- [x] 구독 관리 버튼으로 billing 화면에 진입할 수 있다.
