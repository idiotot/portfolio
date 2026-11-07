+++
title = '개인 포트폴리오'
description = '컴퓨터공학을 공부하는 idiotot의 포트폴리오. Rust로 만드는 LMS 자료 관리 도구 NAVI Study와 개발 과정을 소개합니다.'
+++

# Rust로 학습 자료 관리 도구를 개발하고 있습니다.

컴퓨터공학을 전공하는 **idiotot**입니다. 수업에서는 Java를 공부하고, 개인 프로젝트에서는 Rust로 강의 자료와 과제 일정을 관리하는 프로그램을 만들고 있습니다.

[소개와 학습 내용](about/) · [GitHub](https://github.com/idiotot)

## 관심 분야와 사용 기술

관심 분야는 학습 자료 관리와 데스크톱 프로그램입니다. 현재 프로젝트에 **Rust · SQLite · Iced**를 사용하고 있습니다.

{{< project-preview >}}
## [NAVI Study](projects/lms/)

**2026년 11월 · 개발 중 · 개인 프로젝트**

과목별 자료를 찾고, 과제 마감일의 근거를 확인하는 프로그램입니다. 저장·변환 모듈과 가상 자료로 동작하는 화면을 구현했습니다. 화면과 실제 저장소를 연결하는 작업은 남아 있습니다.

[![NAVI Study의 과목별 자료 목록과 본문 화면](images/navi-archive.png)](projects/lms/)

[프로젝트 상세 보기 →](projects/lms/)
{{< /project-preview >}}

## 개발하면서 다룬 문제

같은 과제의 본문과 마감 정보에 서로 다른 날짜가 있을 수 있습니다. NAVI Study에서는 날짜를 하나로 고르지 않고, 근거가 충돌하거나 부족한 상태를 구분하도록 구현했습니다.

[마감일 판정과 UI 상태 처리](projects/lms/#문제-해결) · [코드 발췌와 테스트 기록](projects/lms/implementation/)
