+++
title = '개인 포트폴리오'
description = '컴퓨터공학 전공 학습과 Rust 기반 개인 프로젝트 NAVI Study의 설계·구현·검증 기록.'
+++

# Rust 기반 학습 자료 관리 도구 개발

**idiotot · 컴퓨터공학 전공**

Java 기반 객체지향 프로그래밍을 학습하고 있으며, 개인 프로젝트로 Rust 기반 학습 자료 관리 도구인 NAVI Study를 개발하고 있다.

[소개 및 학습 내용](about/) · [GitHub](https://github.com/idiotot)

## 관심 분야 및 적용 기술

주요 관심 분야는 학습 자료 관리와 데스크톱 응용프로그램 개발이다. 현재 프로젝트에는 **Rust · SQLite · Iced**를 적용하였다.

{{< project-preview >}}
## [NAVI Study](projects/lms/)

**2026년 11월 · 개발 중 · 개인 프로젝트**

과목별 강의 자료 탐색과 과제 마감 근거 확인을 지원하는 데스크톱 프로그램이다. 자료 저장·변환 모듈과 가상 데이터 기반 GUI 프로토타입을 구현하였다. 실제 자료 조회 및 영구 저장 기능의 GUI 연계는 후속 개발 범위에 해당한다.

[![NAVI Study의 과목별 자료 목록과 본문 화면](images/navi-archive.png)](projects/lms/)

[프로젝트 상세 보기 →](projects/lms/)
{{< /project-preview >}}

## 주요 구현 과제

과제 본문에 기재된 일자와 등록된 마감 정보가 일치하지 않는 경우를 처리하기 위해, 변환 결과에 근거 문장과 마감 판정 상태를 함께 제시하도록 구현하였다.

[마감 판정 및 UI 상태 처리](projects/lms/#문제-해결) · [코드 발췌 및 검증 기록](projects/lms/implementation/)
