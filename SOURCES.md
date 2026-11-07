# 출처와 제작 범위

## 테마와 수업 제공 자료

- Jan Raasch, Hugo Bear Blog: https://github.com/janraasch/hugo-bearblog
- 테마 안내: https://themes.gohugo.io/themes/hugo-bearblog/
- 원본 취득: 2026-11-07, master의 Download ZIP.
- MIT 라이선스: 루트 `LICENSE`와 `site/themes/template/LICENSE`의 원본 고지를 보존했습니다.
- 수업 제공 `prepare_github_pages.py` 3.1을 실행해 `site/`와 배포 파일을 생성했습니다. `.github/pages_build.py`는 같은 스크립트의 복사본이며 이번 개선에서 두 파일을 수정하지 않았습니다.
- 워크플로는 현재 테마가 쓰는 Hugo·Python만 설치하도록 줄였습니다. `--dependencies`의 범용 CSS 도구 설치는 호출하지 않습니다.

## 직접 제작 프로젝트와 화면

NAVI Study 개인 프로젝트의 가상 자료 프로토타입을 실행해 만든 기존 PNG를 재사용했습니다. 이미지 내용을 생성하거나 수정하지 않았습니다.

| 사용 위치 | 원본 프로젝트의 상대 경로 |
| --- | --- |
| `site/static/images/navi-archive.png` | `artifacts/ui/archive.png` |
| `site/static/images/navi-schedule.png` | `artifacts/ui/schedule.png` |
| `site/static/images/navi-evidence.png` | `artifacts/ui/evidence.png` |

실제 강의 첨부파일·로그인 정보는 공개 화면에 포함하지 않았습니다. GUI 화면과 실제 저장소 연결 상태를 구분했습니다.

## 코드와 검증 근거

`site/content/projects/implementation.md`에는 로컬 NAVI Study 소스의 짧은 코드 발췌와 검증 내용을 실었습니다. 파일별 검토 목록과 SHA-256은 `docs/navi-implementation-review.md`에 있습니다. 전체 앱 소스의 공개 저장소 URL은 확인되지 않아 임의의 링크를 넣지 않았습니다.

이번에 `cargo test --offline --workspace -- --test-threads=1`을 실행해 저장 26 + 변환 11 + GUI 상태 5 = 42개 통과를 확인했습니다. 기존 GUI 실행 캡처와 이번 자동 테스트 결과를 구분했습니다.

## AI 지원과 외부 의존성

Codex를 사용해 과제 조건·로컬 소스 확인, 한국어 콘텐츠 수정, CSS 정리, Hugo 설정, 브라우저 검사와 문서 정리를 진행했습니다. Rust·Iced의 개인적인 선택 계기와 확인되지 않은 성과는 만들어 넣지 않았습니다.

AI 생성 이미지, 외부 웹폰트, 외부 아이콘, 분석 스크립트와 별도 UI 라이브러리는 사용하지 않았습니다. 시스템 글꼴과 텍스트 링크를 사용합니다.
