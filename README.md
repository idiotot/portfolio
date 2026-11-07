# idiotot 개인 포트폴리오

Hugo Bear Blog를 사용해 제작한 개인 소개 사이트입니다. 소개, NAVI Study 개인 프로젝트, 기술과 학습 목표를 담았습니다.

- 사이트: https://idiotot.github.io/portfolio/
- 저장소: https://github.com/idiotot/portfolio
- 소개: https://idiotot.github.io/portfolio/about/
- 프로젝트: https://idiotot.github.io/portfolio/projects/lms/

## 환경과 설치

- Hugo Extended 0.167.0: https://github.com/gohugoio/hugo/releases/tag/v0.167.0
- Python 3.9 이상. 로컬 검증에 사용한 Python 버전은 검증 기록을 참조하세요.
- Git, UTF-8 터미널
- 테마 소스는 site/themes/template에 포함되어 있어 submodule 초기화가 필요 없습니다.
- 이 테마의 빌드에 별도의 npm 의존성 설치는 필요 없습니다.

## 빌드와 확인

저장소 루트에서 실행합니다. hugo와 python3가 PATH에 있어야 합니다.

```bash
hugo version
python3 --version
python3 .github/pages_build.py --dependencies
python3 .github/pages_build.py --build
hugo server --source site --config hugo.toml,pages.json --baseURL http://localhost:1313/portfolio/ --bind 127.0.0.1
```

마지막 명령이 안내하는 http://localhost:1313/portfolio/ 에 접속합니다. 배포 빌드 결과는 public/에 생성되며 Git에 저장하지 않습니다.

## 수정과 재배포

- site/content/_index.md: 홈 소개와 기술·학습 목표
- site/content/about.md: 관심 분야, 역량, 다음 행동
- site/content/projects/lms.md: 대표 활동의 기간·목표·역할·과정·결과·한계
- site/static/images/navi-*.png: 직접 실행한 가상 자료 프로토타입 화면
- site/hugo.toml: 사이트 설명, 언어, 메뉴
- site/pages.json: 수업 스크립트가 생성한 운영 baseURL과 테마 설정
- site/layouts/partials/custom_head.html: 반응형 스타일과 초점 표시
- site/layouts/partials/nav.html: 저장소 하위 경로를 반영한 메뉴
- .github/workflows/hugo-pages.yml: main push로 빌드·Pages 배포

GitHub Settings → Pages → Build and deployment → Source를 **GitHub Actions**로 설정합니다.

```bash
git add site README.md SOURCES.md
git commit -m "Describe the actual change"
git push origin main
```

Actions 탭에서 해당 SHA의 build와 deploy 성공을 확인하고 공개 주소에서 변경을 확인합니다. 최초 배포 후 실제 내용을 변경해 push하고 화면에 반영되는지도 확인합니다.

## 검증과 출처

1440px·768px·390px에서 가로 넘침, 이미지, 메뉴를 확인합니다. 내부 주소 직접 접근·새로고침·뒤로 가기와 Tab·Enter 조작을 확인합니다. 결과와 남은 한계는 VALIDATION.md에 기록합니다.

출처와 AI 지원 범위는 [SOURCES.md](SOURCES.md), 테마 원본 안내는 [THEME_README.md](THEME_README.md), 라이선스는 [LICENSE](LICENSE)를 참고하세요. 보고서와 학번·실명은 공개 저장소에 포함하지 않습니다.
