# pwcamp26 — 2026 파워웨이브 캠프

바닐라 HTML/CSS/JS 3파일로 구성된 캠프 운영 웹앱. 프레임워크·빌드 과정 없음, GitHub Pages로 정적 배포.

## 파일 구성

| 파일 | 용도 | 접근 대상 |
|---|---|---|
| `index.html` | 교사용 기획 대시보드 (읽기 전용) + 전체 페이지 링크 허브 | 프로그램팀 교사 |
| `mission.html` | 학생용 미션 페이지 (조 선택 → 코드 잠금해제 → 미션1 사진 → 미션2 보고서) | 학생 (조당 1명) |
| `admin.html` | 실시간 제출 현황 모니터링 + 준비 소식 의견함 모더레이션 (비밀번호: `pwave2026`) | 관리자 |
| `teacher.html` | 교사용 실행 매뉴얼 (타임테이블·역할표·비상연락망·조별명단·정책) | 전체 교사 |
| `station.html` | Day2 6관문+Final 담당자 카드 (게임/30초 멘트/통과기준/안전/준비물) | 관문 담당 교사 |
| `emergency.html` | 비상 대응표 8종 (초안, 프로그램팀 검토 필요) | 전체 교사 |
| `checklist.html` | 관문별 준비물·담당자·세팅/회수 체크리스트 | 프로그램팀 교사 |
| `news.html` | 준비 소식 + 의견함(댓글). 댓글 작성엔 선생님 코드(`pwave2026`, admin.html과 동일값) 필요 | 전체 교사 |

- 배포: `main` push 시 `.github/workflows/deploy.yml` → GitHub Pages 자동 배포. 빌드 스텝 없음, 저장소 루트를 그대로 올림.
- 데이터: Firebase Realtime DB (`pwcamp26-default-rtdb`). 경로: `submissions/team{N}/mission{1,2}`(학생 제출, 사진은 Storage 대신 압축 후 base64로 DB 직접 저장), `checklist/{itemId}`(준비물 체크), `teacherDoc/{roles|roster|policy}`(교사 매뉴얼 공동편집), `news_comments/{newsId}`(의견함 댓글).
- `mission.html`의 `TEAM_CODES`에 조별 비밀번호 하드코딩. 힌트는 페이지에 노출하지 않고 교사가 별도 전달.
- `localStorage` 키 `pwcamp_unlocked_{team}`으로 조별 잠금해제 상태 유지.
- Firebase 규칙: `submissions`/`checklist`/`teacherDoc`/`news_comments` 네 경로 모두 read/write 허용 (2026-07-08 콘솔에 반영 완료, `firebase.rules.now.json` 참고). 새 최상위 경로를 추가로 쓰려면 이 규칙에 없는 경로는 기본 거부이므로 콘솔에서 함께 열어줘야 함.

## 요구사항/백로그

**`REQUIREMENTS.md`가 단일 소스**. 새 요구사항, 미결 사항, 우선순위는 항상 이 파일에서 확인·갱신할 것. 코드를 보고 요구사항을 추측하지 말고 REQUIREMENTS.md를 먼저 읽는다.

## 현재 구현 상태 (2026-07-08 기준, origin/main에 push 완료)

- 완료(REQ-01~24, REQUIREMENTS.md "완료된 기능" 참고): 조별 코드 잠금해제·고정, 미션1→미션2 순차 해금, 관리자 승인/반려 기반 진행, 목적지 배너 단계적 공개, 미션 전환 스토리텔링, Day1 목적문구·보고서 3항목(장벽/믿음/돌파기도), Day2 6관문+Final 구조, 개인정보 안내, 운영 페이지 5종(teacher/station/emergency/checklist/news) 신규 — Firebase 저장까지 실제 브라우저로 재검증 완료
- 미구현: 비밀번호 해금 직후 → 미션1 등장 전 인트로 내레이션 없음 (REQ-09에는 미션1→미션2 전환 연출만 반영됨)

⚠️ **작업 시작 전 항상 `git fetch && git status`로 origin 대비 뒤처지지 않았는지 먼저 확인할 것.** 이 프로젝트는 다른 세션/사람이 병행으로 직접 push하는 경우가 있어, 로컬이 stale한 상태로 요구사항을 재작성하면 이미 구현된 것을 다시 요청하거나 REQ 번호가 충돌한다.
