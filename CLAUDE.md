# pwcamp26 — 2026 파워웨이브 캠프

바닐라 HTML/CSS/JS 3파일로 구성된 캠프 운영 웹앱. 프레임워크·빌드 과정 없음, GitHub Pages로 정적 배포.

## 파일 구성

| 파일 | 용도 | 접근 대상 |
|---|---|---|
| `index.html` | 교사용 기획 대시보드 (읽기 전용) | 프로그램팀 교사 |
| `mission.html` | 학생용 미션 페이지 (조 선택 → 코드 잠금해제 → 미션1 사진 → 미션2 보고서) | 학생 (조당 1명) |
| `admin.html` | 실시간 제출 현황 모니터링 (비밀번호: `pwave2026`) | 관리자 |

- 배포: `main` push 시 `.github/workflows/deploy.yml` → GitHub Pages 자동 배포. 빌드 스텝 없음, 저장소 루트를 그대로 올림.
- 데이터: Firebase Realtime DB (`pwcamp26-default-rtdb`), 경로 `submissions/team{N}/mission{1,2}`. 사진은 Storage가 아니라 압축 후 base64로 DB에 직접 저장 (무료 플랜 대응).
- `mission.html`의 `TEAM_CODES`에 조별 비밀번호 하드코딩. 힌트는 페이지에 노출하지 않고 교사가 별도 전달.
- `localStorage` 키 `pwcamp_unlocked_{team}`으로 조별 잠금해제 상태 유지.

## 요구사항/백로그

**`REQUIREMENTS.md`가 단일 소스**. 새 요구사항, 미결 사항, 우선순위는 항상 이 파일에서 확인·갱신할 것. 코드를 보고 요구사항을 추측하지 말고 REQUIREMENTS.md를 먼저 읽는다.

## 현재 구현 상태 (origin/main `e390da9` 기준, 2026-07-08)

- 완료(REQ-01~14, REQUIREMENTS.md "완료된 기능" 참고): 조별 코드 잠금해제·고정, 미션1→미션2 순차 해금, **관리자 승인/반려 기반 진행**(제출 → pending → admin.html에서 승인/반려 → 승인 시에만 다음 단계, 반려 시 재제출), 목적지 배너 단계적 공개 + 갱신 강조 애니메이션, 미션 전환 스토리텔링, 기기 초기화 기능, 사진 입력 카메라/갤러리 둘 다 허용
- 미구현으로 남아있는 것: 비밀번호 해금 직후 → 미션1 등장 전 인트로 내레이션 없음 (`checkCode()` 성공 시 바로 `showMissions()` 호출). REQ-09에는 미션1→미션2 전환 연출만 반영됨

⚠️ **작업 시작 전 항상 `git fetch && git status`로 origin 대비 뒤처지지 않았는지 먼저 확인할 것.** 이 프로젝트는 다른 세션/사람이 병행으로 직접 push하는 경우가 있어, 로컬이 stale한 상태로 요구사항을 재작성하면 이미 구현된 것을 다시 요청하거나 REQ 번호가 충돌한다.
