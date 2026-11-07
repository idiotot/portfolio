# 출처 및 제작 범위

## 테마 및 수업 제공 자료

- Jan Raasch, Hugo Bear Blog: https://github.com/janraasch/hugo-bearblog
- 테마 안내: https://themes.gohugo.io/themes/hugo-bearblog/
- 원본 취득: 2026-11-07, master 브랜치의 Download ZIP.
- 이용 조건은 MIT 라이선스이며, 루트 `LICENSE`와 `site/themes/template/LICENSE`의 고지를 보존하였다.
- 수업 제공 `prepare_github_pages.py` 3.1을 실행하여 `site/`와 배포 파일을 생성하였다. `.github/pages_build.py`는 동일한 스크립트의 복사본이다. 이번 개선에서 두 파일의 코드는 수정하지 않았다.
- 워크플로는 현재 테마에 필요한 Hugo와 Python만 설치하도록 정리하였다. 범용 CSS 도구를 설치하는 `--dependencies` 단계는 호출하지 않는다.

## 프로젝트 및 실행 화면

NAVI Study 개인 프로젝트의 가상 데이터 기반 프로토타입을 실행하여 생성한 기존 PNG를 재사용하였다. 이미지 내용의 생성 또는 수정은 수행하지 않았다.

| 사용 위치 | 원본 프로젝트의 상대 경로 |
| --- | --- |
| `site/static/images/navi-archive.png` | `artifacts/ui/archive.png` |
| `site/static/images/navi-schedule.png` | `artifacts/ui/schedule.png` |
| `site/static/images/navi-evidence.png` | `artifacts/ui/evidence.png` |

공개 화면에는 실제 강의 첨부파일과 로그인 정보를 포함하지 않았다. 가상 데이터 기반 GUI와 실제 저장 모듈의 연계 상태를 구분하여 서술하였다.

## 코드 및 검증 근거

`site/content/projects/implementation.md`에는 로컬 NAVI Study 소스의 일부와 검증 결과를 제시하였다. 검토 파일 목록 및 SHA-256은 `docs/navi-implementation-review.md`에 기록하였다. 전체 앱 소스의 공개 저장소 URL은 확인되지 않아 임의의 링크를 추가하지 않았다.

2차 검토에서는 2026-11-07 20:13:28~20:13:32 KST, CachyOS Linux x86_64 / Rust·Cargo 1.99.1 환경에서 `cargo test --offline --locked --workspace -- --test-threads=1`을 실행하였다. 저장 계층 26개, 변환기 11개 및 GUI 상태 5개를 합산하여 42개 통과를 확인하였다. 기존 GUI 실행 기록과 이번 자동 테스트 결과는 별도로 구분하였다. GUI 5개는 창을 띄우지 않는 상태 처리 검증이며, 저장·변환 모듈에는 임시 DB·파일시스템 연계 검증이 포함된다. `site/static/evidence/`에 절대 경로를 정규화한 로그와 환경·소스 해시를 공개하였다. 전체 앱 소스가 공개되지 않아 포트폴리오만으로 독립 재현할 수 없다는 조건도 명시하였다.

## AI 지원 및 언어 선정 근거

Codex를 활용하여 과제 조건과 로컬 소스를 대조하고, 콘텐츠 수정·CSS 정리·Hugo 설정·브라우저 검사 및 문서 작성을 수행하였다. Rust 및 Iced 선정 근거와 데스크톱 UI의 초기 기획 배경은 작성자가 직접 제공한 내용을 반영하였다. Iced의 메시지 기반 상태 처리와 Rust workspace 통합은 실제 소스와 대조하였다. 비교 성능, 모바일 대비 우위 및 확인되지 않은 개발 성과는 추가하지 않았다.

Rust의 언어 특성은 다음 공식 문서를 참고하였다.

- 소유권 및 GC 없는 메모리 관리: https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html
- 소유권·타입 검사와 동시성: https://doc.rust-lang.org/book/ch16-00-concurrency.html
- Cargo의 패키지·의존성·프로젝트 구성: https://doc.rust-lang.org/cargo/guide/

AI 생성 이미지, 외부 웹폰트, 외부 아이콘, 분석 스크립트 및 별도 UI 라이브러리는 사용하지 않았다. 글꼴은 시스템 기본 글꼴을 사용하며, 링크는 텍스트로 제공한다.
