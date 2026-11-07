# idiotot 개인 포트폴리오

컴퓨터공학 학습과 Rust 개인 프로젝트 NAVI Study를 소개하는 Hugo 사이트입니다. Hugo Bear Blog 테마와 GitHub Pages 배포를 유지하고, 실제 코드에서 확인한 구현 범위와 학습 내용을 정리했습니다.

- 홈: https://idiotot.github.io/portfolio/
- 소개: https://idiotot.github.io/portfolio/about/
- 프로젝트: https://idiotot.github.io/portfolio/projects/lms/
- 구현 노트: https://idiotot.github.io/portfolio/projects/lms/implementation/
- 저장소: https://github.com/idiotot/portfolio

## 환경과 빌드

Hugo **Extended 0.167.0**, Python **3.9 이상**, Git이 필요합니다. 이번 로컬 검증은 Python 3.14.9와 Hugo Extended 0.167.0을 사용했습니다.

Hugo 설치 파일: https://github.com/gohugoio/hugo/releases/tag/v0.167.0

테마는 `site/themes/template/`에 포함되어 있습니다. 현재 사이트는 Go module, Sass, npm, 외부 CSS/JS 도구를 사용하지 않습니다.

저장소 루트에서 실행합니다. `hugo`와 `python3`가 PATH에 있어야 합니다.

```bash
hugo version
python3 --version
python3 .github/pages_build.py --build
```

결과는 `public/`에 생성됩니다. 로컬 미리보기:

```bash
hugo server --source site --config hugo.toml,pages.json --baseURL http://localhost:1313/portfolio/ --bind 127.0.0.1
```

브라우저에서 http://localhost:1313/portfolio/ 를 엽니다.

## 실제 수정할 파일

| 위치 | 역할 |
| --- | --- |
| `site/content/_index.md` | 개인 소개, 관심 분야, 대표 프로젝트 카드 |
| `site/content/about.md` | 전공 학습, 사용한 기술과 학습 중인 내용 |
| `site/content/projects/lms.md` | 개요·구현 범위·문제 해결·테스트·한계·계획 |
| `site/content/projects/implementation.md` | 실제 코드 발췌와 테스트 설명 |
| `site/assets/css/portfolio.css` | 공통 색상·문서·메뉴·카드·이미지·코드·반응형 스타일 |
| `site/layouts/partials/custom_head.html` | Hugo 내장 minify·fingerprint로 CSS 연결 |
| `site/layouts/partials/nav.html` | 내부 메뉴 경로와 현재 페이지 표시 |
| `site/layouts/_default/` | 한국어 문서 구조와 페이지 h1 |
| `site/layouts/shortcodes/project-preview.html` | 홈의 프로젝트 영역 |
| `site/layouts/shortcodes/screenshot.html` | 실제 이미지·설명·원본 링크 |
| `site/static/images/navi-*.png` | 실제 가상 자료 프로토타입 실행 화면 3장 |
| `site/hugo.toml` | 제목·언어·메뉴·설명 |
| `site/pages.json` | 수업 스크립트가 생성한 운영 baseURL과 테마 설정 |
| `docs/navi-implementation-review.md` | 로컬 NAVI 소스 검토 목록·해시·테스트 결과 요약 |

기존 URL과 About·프로젝트의 주요 앵커는 유지했습니다. 코드와 표의 가로 스크롤은 해당 영역 안에서만 발생합니다.

## 원본 및 수업 제공 파일

- `site/themes/template/`: 보존한 Bear Blog 원본. 개인화는 `site/layouts/` 오버라이드와 `site/assets/`에서 합니다.
- 루트의 `layouts/`, `exampleSite/`, `images/`, `theme.toml` 등: Download ZIP으로 받은 테마 원본. 운영 빌드의 source는 `site/`입니다.
- `THEME_README.md`, `LICENSE`: 테마 원본 안내와 라이선스 고지.
- `prepare_github_pages.py`, `.github/pages_build.py`: 수업 제공 스크립트 3.1 원본과 배포용 복사본. 이번 개선에서 코드를 수정하지 않았습니다.
- 루트와 테마의 `package.json`: 원본 테마 개발용 자료이며 현재 사이트 빌드에는 사용하지 않습니다.

범용 스크립트의 `--dependencies`는 사용하지 않는 CSS 도구까지 설치합니다. 현재 Bear Blog 구성에는 필요하지 않아 CI에서 호출하지 않습니다. 다른 테마나 빌드 도구를 도입하면 의존성 설치 절차를 다시 검토해야 합니다.

## 수정과 재배포

GitHub Settings → Pages → Build and deployment → Source를 **GitHub Actions**로 설정합니다.

```bash
git add site README.md SOURCES.md VALIDATION.md docs
git commit -m "Describe the actual change"
git push origin main
```

Actions에서 해당 SHA의 `build`·`deploy` 성공을 확인하고 공개 주소에서 변경을 확인합니다. 워크플로는 Hugo와 Python만 설치하고, 수업 제공 빌드 스크립트와 Pages artifact·deploy 단계를 사용합니다. CSS는 Hugo 내장 자산 처리로 생성하며 브라우저에서 무결성 해시를 검사합니다.

## 검증과 출처

[VALIDATION.md](VALIDATION.md)에 브라우저·빌드·링크·접근성 검사와 한계를, [SOURCES.md](SOURCES.md)에 테마·이미지·코드 발췌·AI 지원 범위를 기록합니다. NAVI Study는 가상 자료 GUI와 별도의 저장 모듈을 구분해 설명합니다.

보고서와 학번·실명은 공개 저장소에 포함하지 않습니다. `public/`, `site/public/`, 캐시와 임시 도구는 Git에서 제외합니다.
