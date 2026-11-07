# 기능 검증 기록

검사일: 2026-11-07 KST. 브라우저: Codex 내장 브라우저, 버전 미표시. OS: CachyOS Linux.

## 빌드와 실행

- Hugo Extended 0.167.0과 Python 3.14.9로 수업 제공 .github/pages_build.py --build를 실행해 public/index.html 생성과 빌드 성공을 확인했다.
- README의 hugo server 명령으로 별도 로컬 서버를 띄우고 /portfolio/에서 HTTP 200을 확인했다. 검사가 끝난 서버는 종료했다.
- 생성된 모든 HTML의 내부 링크·이미지 참조 38개를 파일 경로와 대조해 누락이 없음을 확인했다.
- 최초 main push SHA: 52a7af98c8af15bb5569ba938d2c70182bec61f9
- 최초 push 실행: https://github.com/idiotot/portfolio/actions/runs/42361332077
- 위 실행의 Hugo build와 GitHub Pages deploy가 모두 성공했다. 공개 홈에서 본문·메뉴·이미지 로딩을 확인했다.
- 후속 변경은 홈에 프로토타입의 가상 자료·구현 범위를 명확히 적은 것이다. 후속 push의 배포 결과와 최종 SHA는 제출 보고서의 Actions 링크를 기준으로 확인한다.

## T01 배포

예상: main push가 Actions를 시작하고 성공한 빌드 결과가 Pages에 배포되어야 한다.
실제: 최초 실행의 event=push, build=success, deploy=success와 공개 페이지 접속을 확인했다. 최종 제출 버전은 후속 push의 SHA와 Actions를 보고서에 맞춘다.

## T02 경로와 자산

예상: https://idiotot.github.io/portfolio/projects/lms/ 에 직접 접근·새로고침해도 내용과 이미지가 표시되어야 한다.
실제: 로컬 운영 빌드에서 내부 주소·새로고침과 두 이미지 로딩을 확인했다. 공개 홈의 메뉴·이미지는 /portfolio/ 경로와 HTTPS를 유지했다. 모든 이미지는 alt가 있다.

## T03 메뉴와 링크

예상: 소개·프로젝트·홈·학습 목표 앵커와 이미지 원본·본인 GitHub 링크가 목적지로 연결되어야 한다.
실제: 브라우저에서 메뉴 이동, 내부 새로고침, 뒤로 가기를 확인했다. GitHub 링크는 https://github.com/idiotot 이다. 빈 href와 의미 없는 # 링크를 넣지 않았다.

## T04 반응형

예상: 뷰포트 1440px·768px·390px에서 본문이 가로로 넘치지 않고 메뉴와 이미지가 보인다.
실제: 세 폭 모두 documentElement.scrollWidth가 innerWidth를 넘지 않았다. 390px의 프로젝트 페이지도 넘침 없이 두 이미지가 로드되었다. 폭 변경에 JavaScript 메뉴 상태를 사용하지 않아 숨김 상태가 남지 않았다.

## T05 키보드와 문서 구조

예상: Tab으로 조작에 도달하고 초점이 보이며 Enter로 실행할 수 있어야 한다.
실제: Tab으로 본문 건너뛰기 링크에 도달했고 solid 윤곽선을 확인했다. Enter 뒤 URL의 #main과 main 초점을 확인했다. lang=ko, 페이지별 h1과 하위 제목을 확인했다.

## 관찰과 수정 후 재검증

내부 페이지의 h1이 없는 것을 관찰해 테마 single.html의 blog 전용 제목 조건을 확인했다. 운영용 single.html에서 모든 페이지에 제목을 출력하도록 바꾼 뒤 NAVI Study h1이 표시되는 것을 다시 확인했다.

한국어 제목의 어미만 다음 줄로 넘어가는 현상을 보고 keep-all과 text-wrap:balance를 적용했다. 재빌드·새로고침 후 390px에서 가로 넘침이 없었다. README 실행 검증 중 site/public/ 생성도 확인해 .gitignore에 추가했다.

## 확인 범위와 한계

200% 브라우저 확대, 다른 브라우저·OS, 화면 낭독기는 별도 검증하지 않았다. NAVI Study 프로젝트 화면은 가상 자료 프로토타입이며 실제 서비스 완료나 온라인 인증·동기화 구현의 증거로 사용하지 않는다.

유효 커밋은 원본 ZIP 단순 등록을 제외한 실제 변경만 집계한다. 공개 기준 P51의 50개 이상에는 미달하며 정확한 최종 개수와 제외 SHA는 보고서에 기재한다.
