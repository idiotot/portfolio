# 기능 검증 기록

2026-11-07 KST. CachyOS Linux / Hugo Extended 0.167.0 / Python 3.14.9 / Codex 내장 브라우저(버전 미표시).

## 이번 개선의 빌드와 정적 검사

- 수업 제공 `.github/pages_build.py --build` 실행 성공. HTML 파일 6개를 생성했다.
- 생성 HTML의 내부 참조 60개, 앵커 참조 16개에 경로·대상 누락이 없었다.
- CSS의 무결성 해시(SRI)를 사용하는 참조 6개가 실제 생성 파일의 SHA-256과 일치했다.
- 홈·소개·프로젝트·구현 노트는 h1이 각각 1개이고 lang=ko다.
- 운영 HTML에 localhost·127.0.0.1·학번·실명이 남지 않았고 지적된 추상적 소개 문장을 제거했다.
- 테마 원본·라이선스와 수업 제공 스크립트를 보존했다. `prepare_github_pages.py`와 `.github/pages_build.py`는 cmp로 동일함을 확인했다.

## 브라우저 검사

홈·소개·프로젝트·구현 노트 4개 화면을 1440px·768px·390px에서 확인했다.

| 검사 | 예상 | 실제 결과 |
| --- | --- | --- |
| 경로·CSS | /portfolio/ 하위 경로에서 CSS·내부 페이지 로딩 | 각 화면에서 CSS 1개 적용, 경로 유지 |
| 반응형 | 본문·메뉴가 가로로 넘치지 않음 | 12개 화면 조합 모두 scrollWidth가 innerWidth를 넘지 않음 |
| 코드·표 | 긴 코드는 해당 영역 내부에서 스크롤 | 390px 구현 노트에서 본문 넘침 없이 코드 영역에 가로 스크롤 |
| 메뉴 | 모바일에서도 선택 가능 | 세 메뉴 모두 높이 44px, 화면 범위 안에 표시 |
| 이미지 | 원본 비율·alt 유지, 필요한 시점에 로딩 | 스크롤 후 3장 모두 1360×860으로 로드, alt 확인 |
| 키보드 | Tab 초점·Enter 실행·본문 이동 | 초점 윤곽선 3px solid, 소개 메뉴 Enter 이동, Enter 후 main 초점 확인 |
| 현재 위치 | 색상 외 표시로 구분 | About 메뉴에 aria-current=page와 밑줄 표시 |
| 대비 | 주요 글·링크 4.5:1 이상 | 본문 8.36:1, 링크 7.29:1, 보조 설명 6.19:1 |

스크린샷 아래 원본 이미지 링크로 전체 크기를 열 수 있다. 지연 로딩된 화면 아래 이미지는 첫 로딩 시 naturalWidth가 0일 수 있어 실제로 스크롤한 뒤 로딩을 확인했다.

## GitHub Actions와 배포

- main push 자동 실행, Pages의 GitHub Actions 배포 방식을 유지했다.
- CI에서 이 테마가 사용하지 않는 Node·Go·Dart Sass와 범용 CSS 도구 설치 단계를 제거했다.
- Hugo 0.167.0, Python, 수업 제공 빌드 스크립트와 Pages artifact·deploy 단계를 사용한다.
- 최초 배포 성공 기록: https://github.com/idiotot/portfolio/actions/runs/42361332077
- 이번 변경의 실제 실행 상태: https://github.com/idiotot/portfolio/actions/workflows/hugo-pages.yml
- 제출 보고서에는 최종 main 전체 SHA와 그 SHA의 성공 실행 URL을 기록한다.

## NAVI Study 설명의 검증

로컬 프로젝트에서 `cargo test --offline --workspace -- --test-threads=1`을 실행했다. 저장 26개 + 변환 11개 + GUI 상태 5개 = 42개 통과, 실패 0개다. child 보조 실행은 중복 합산하지 않았다.

SQLite 모듈 구현과 GUI의 실제 저장 기능을 구분했다. 중복·마감 판정·메모리 UI 상태를 코드에서 확인했으며 `docs/navi-implementation-review.md`에 검토 파일의 SHA-256과 결과 요약을 기록했다. GUI 이미지 3장은 기존 실행 화면을 재사용했다. 새 GUI 실행이나 실제 LMS 연결을 검증했다고 쓰지 않았다.

## P51 유효 커밋 기준 확인

`2026_과제1_공개채점기준.pdf`의 P51과 붙임 2에 **유효 커밋 50개 이상** 요건이 실제로 있다. 공개 배점은 1점이며 비공개 평가에서도 커밋 이력을 추가 확인한다.

현재 이력은 50개 미만으로 미충족이다. 최초 원본 ZIP·스크립트 단순 등록 커밋 `e91058df38767f6d9cdd98708b83b97edcc9795d`를 제외하고 실제 수정만 센다. 이번 개선은 콘텐츠·CSS·배포 구성·문서 단위로 커밋했다. 최종 main의 정확한 후보·제외·유효 개수는 제출 PDF와 개선 결과 보고서에 기록한다.

```bash
git rev-list --count main
git log --reverse --format='%H %s' main
```

## 남은 확인 범위

- 정확한 200% 브라우저 확대는 내장 브라우저에서 배율 변경을 확인하지 못해 미검증으로 남긴다. 작은 화면 폭 검사는 확대 검사로 대체했다고 쓰지 않는다.
- 다른 브라우저·화면 낭독기와 NAVI 앱의 Windows·macOS 실행은 미검증이다.
- Rust 선정 이유는 작성자가 직접 제공한 내용을 반영했다. Iced를 처음 선택한 개인적인 이유와 별도 앱 공개 저장소 URL은 추가 확인이 필요하다.
