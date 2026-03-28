## Spec: Remote Smoke Test Results

**유형**: `API Endpoint`  
**위치**: `Clinet/api/remote-smoke-test-results.md`  
**작성일**: 2026-03-28  
**상태**: `Blocked by Missing Backend`

---

### 최신 실행

- 실행 일시: 2026-03-28 20:51 KST
- 실행 명령: `node .\scripts\remoteSmokeTest.mjs --base-url http://localhost:4000`
- 결과: 실패

---

### 관찰 내용

1. `http://localhost:4000/` 는 Vite dev server HTML을 반환했다.
2. `POST http://localhost:4000/api/v1/auth/login` 는 `404`를 반환했다.
3. 따라서 2026-03-28 현재 로컬 `4000` 포트에는 프론트 앱만 떠 있고, smoke test 대상 backend는 연결되어 있지 않다.

---

### 단계별 결과

| 단계 | 상태 | 메모 |
| --- | --- | --- |
| 로그인 | Failed | `POST /api/v1/auth/login` -> `404` |
| 세션 복구 | Skipped | 로그인 실패로 미실행 |
| 대시보드 조회 | Skipped | 로그인 실패로 미실행 |
| habit 조회 | Skipped | 로그인 실패로 미실행 |
| habit 체크 | Skipped | 로그인 실패로 미실행 |
| 일정 조회 | Skipped | 로그인 실패로 미실행 |
| 일정 생성 | Skipped | 로그인 실패로 미실행 |
| 일정 수정 | Skipped | 로그인 실패로 미실행 |
| 일정 삭제 | Skipped | 로그인 실패로 미실행 |
| 로그아웃 | Skipped | 로그인 실패로 미실행 |

---

### Fixture 반영 상태

- 실제 backend 응답을 받지 못해서 fixture JSON은 변경하지 않았다.
- 이번 실행으로 확인된 것은 “현재 로컬 4000 포트는 backend가 아니다”라는 점이다.

---

### 다음 실행 조건

- 실제 `/api/v1` backend origin 확보
- `VITE_API_BASE_URL` 또는 `--base-url` 설정
- 테스트 계정 자격 증명 확보
- 필요 시 `npm.cmd run smoke:remote -- --base-url <backend-origin> --capture-fixtures` 재실행
