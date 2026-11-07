# idiotot 개인 포트폴리오

컴퓨터공학 전공 학습과 Rust 기반 개인 프로젝트 NAVI Study의 설계·구현·검증 내용을 정리한 Hugo 사이트이다. Hugo Bear Blog 테마와 GitHub Pages 배포 구조를 유지하였으며, 실제 소스에서 확인한 구현 범위를 중심으로 구성하였다.

- 홈: https://idiotot.github.io/portfolio/
- 소개: https://idiotot.github.io/portfolio/about/
- 프로젝트: https://idiotot.github.io/portfolio/projects/lms/
- 구현 노트: https://idiotot.github.io/portfolio/projects/lms/implementation/
- 저장소: https://github.com/idiotot/portfolio

## 실행 환경 및 빌드

필요 도구는 Hugo **Extended 0.167.0**, Python **3.9 이상** 및 Git이다. 로컬 검증에는 Python 3.14.9와 Hugo Extended 0.167.0을 사용하였다.

Hugo 설치 파일: https://github.com/gohugoio/hugo/releases/tag/v0.167.0

테마는 `site/themes/template/`에 포함되어 있다. 현재 사이트는 Go module, Sass, npm 및 외부 CSS/JS 도구를 사용하지 않는다.

저장소 루트에서 다음 명령을 실행한다. `hugo`와 `python3`는 PATH에 등록되어 있어야 한다.

```bash
hugo version
python3 --version
python3 .github/pages_build.py --build
```

빌드 결과는 `public/`에 생성된다. 로컬 미리보기에는 다음 명령을 사용한다.

```bash
hugo server --source site --config hugo.toml,pages.json --baseURL http://localhost:1313/portfolio/ --bind 127.0.0.1
```

접속 주소는 http://localhost:1313/portfolio/ 이다.

## 운영 소스의 위치 및 역할

| 위치 | 역할 |
| --- | --- |
| `site/content/_index.md` | 소개, 관심 분야 및 대표 프로젝트 카드 |
| `site/content/about.md` | 전공 학습, 적용 기술 및 후속 학습 목표 |
| `site/content/projects/lms.md` | 개발 배경·기술 선정·구현 범위·검증·한계·계획 |
| `site/content/projects/implementation.md` | 코드 발췌 및 테스트 설명 |
| `site/assets/css/portfolio.css` | 색상·문서·메뉴·카드·이미지·코드·반응형 스타일 |
| `site/layouts/partials/custom_head.html` | Hugo 내장 minify·fingerprint를 통한 CSS 연결 |
| `site/layouts/partials/nav.html` | 내부 메뉴 경로 및 현재 페이지 표시 |
| `site/layouts/_default/` | 한국어 문서 구조와 페이지 h1 |
| `site/layouts/shortcodes/project-preview.html` | 홈의 프로젝트 영역 |
| `site/layouts/shortcodes/screenshot.html` | 실제 이미지·설명·원본 링크 |
| `site/static/images/navi-*.png` | 기존 가상 데이터 프로토타입 실행 화면 3장 |
| `site/hugo.toml` | 제목·언어·메뉴·설명 |
| `site/pages.json` | 수업 스크립트가 생성한 운영 baseURL 및 테마 설정 |
| `docs/navi-implementation-review.md` | NAVI 소스 검토 목록·해시·테스트 결과 요약 |

기존 URL과 About·프로젝트의 주요 앵커를 유지하였다. 긴 코드와 표의 가로 스크롤은 해당 영역 내부로 제한하였다.

## 원본 및 수업 제공 파일의 보존

- `site/themes/template/`는 Bear Blog 원본이다. 개인화는 `site/layouts/` 오버라이드와 `site/assets/`에서 수행한다.
- 루트의 `layouts/`, `exampleSite/`, `images/`, `theme.toml` 등은 Download ZIP으로 취득한 테마 원본이다. 운영 빌드의 source는 `site/`이다.
- `THEME_README.md`와 `LICENSE`는 원본 안내 및 라이선스 고지이다.
- `prepare_github_pages.py`와 `.github/pages_build.py`는 수업 제공 스크립트 3.1 원본과 배포용 복사본이다. 이번 개선에서 코드를 수정하지 않았다.
- 루트와 테마의 `package.json`은 원본 테마 개발용 자료이며 현재 사이트의 빌드에는 사용하지 않는다.

범용 스크립트의 `--dependencies`는 사용하지 않는 CSS 도구까지 설치한다. 현재 Bear Blog 구성에서는 해당 의존성이 필요하지 않아 CI의 호출 단계에서 제외하였다. 다른 테마나 빌드 도구를 도입하는 경우에는 의존성 설치 절차를 재검토해야 한다.

## 수정 및 재배포

GitHub Settings → Pages → Build and deployment → Source를 **GitHub Actions**로 설정한다.

```bash
git add site README.md SOURCES.md VALIDATION.md docs
git commit -m "Describe the actual change"
git push origin main
```

Actions에서 대상 SHA의 `build`·`deploy` 성공 여부를 확인하고 공개 주소에서 변경 결과를 확인한다. 워크플로는 Hugo와 Python을 설치한 후 수업 제공 빌드 스크립트, Pages artifact 및 deploy 단계를 실행한다. CSS는 Hugo 내장 자산 처리로 생성하며 브라우저에서 무결성 해시를 검사한다.

## 검증 및 출처

브라우저·빌드·링크·접근성 검사와 한계는 [VALIDATION.md](VALIDATION.md)에, 테마·이미지·코드 발췌와 AI 지원 범위는 [SOURCES.md](SOURCES.md)에 기록하였다. NAVI Study의 가상 데이터 기반 GUI와 별도 저장 모듈의 구현 상태를 구분하여 서술하였다.

보고서와 학번·실명은 공개 저장소에 포함하지 않는다. `public/`, `site/public/`, 캐시 및 임시 도구는 Git에서 제외한다.
