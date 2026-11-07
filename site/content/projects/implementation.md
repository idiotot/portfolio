+++
title = 'NAVI Study 구현 노트'
url = '/projects/lms/implementation/'
description = 'NAVI Study의 저장 구조, 중복 처리, 마감 판정, UI 상태 및 검증 결과.'
+++

2026년 11월 7일 로컬 프로젝트의 소스와 검증 문서를 대상으로 구현 내용을 검토하였다. 파일 경로는 NAVI Study 프로젝트 루트를 기준으로 하며, 코드는 해당 원본 파일의 일부를 발췌하였다.

## 저장 구조 {#저장-구조}

`crates/archive-store/src/schema_v1.sql`의 `records`는 `(kind, source_key)`를 기본키로 사용한다. 자료의 제목, JSON payload, 존재 상태 및 관측 시각을 저장한다.

```sql
PRIMARY KEY(kind,source_key)
```

`watermarks`는 `(course_id, kind)`를 기준으로 마지막 관측 시각과 완전한 목록의 처리 여부를 기록한다.

```sql
CREATE TABLE watermarks (
    course_id TEXT NOT NULL,
    kind TEXT NOT NULL CHECK(kind IN ('course','document','attachment','event')),
    observed_at INTEGER NOT NULL CHECK(observed_at > 0),
    complete INTEGER NOT NULL CHECK(complete IN (0,1)),
    PRIMARY KEY(course_id,kind)
) STRICT;
```

`store.rs`는 레코드와 watermark를 하나의 트랜잭션 내에서 갱신한다. 동일한 관측 시각의 동일 레코드에 대한 재시도는 허용하되, 같은 시각에 내용이 다른 레코드는 충돌로 처리한다. 현재 스키마에는 GUI의 읽음·보관 상태를 저장하는 필드가 없다.

## 자료 중복 처리

검토 파일: `crates/catalog-import/src/lib.rs`, `source.rs`

과목 ID·원본 종류·원본 ID로 구성한 키를 기준으로 문서를 그룹화한다. 동일 키의 문서가 여러 개 존재하면 `DuplicateMerged` 유형을 기록하고 최신 `fetched_at`의 문서를 선택한다. 관측 시각이 동일한 경우 경로와 제목을 비교하여 입력 배열의 순서에 따른 결과 변화를 방지한다.

게시판 글 ID는 게시판 ID와 함께 보존한다. 서로 다른 과목 또는 원본의 자료는 파일 해시가 동일하다는 이유만으로 병합하지 않는다.

## 마감일 판정

검토 파일: `crates/archive-store/src/domain.rs`, `crates/catalog-import/src/evidence.rs`

마감 판정은 다음 세 상태로 표현한다. 아래 발췌에서는 직렬화 관련 속성을 생략하였다.

```rust
pub enum Deadline {
    Unknown,
    Confirmed { value: DeadlineCandidate },
    Conflict { candidates: Vec<DeadlineCandidate> },
}
```

각 후보는 시각과 `Evidence { path, line, quote }`를 보유한다. 완전한 날짜·시각은 시차를 반영한 시점으로 비교한다. 지원 범위 내에서 후보가 하나이면 확정하고, 서로 다른 시각이 남으면 충돌로 분류한다. 부분적인 날짜와 공식 날짜가 일치하지 않거나 후보 수가 처리 상한인 8개를 초과하면 확인 필요 상태를 반환한다.

## Iced GUI 구성 {#gui-구성}

검토 파일: `crates/study-ui/src/main.rs`, `view.rs`, `Cargo.toml`

`Message`는 검색·과목 선택·검토 표시 등의 사용자 입력을 표현한다. `App::update`가 메시지에 따라 `App`의 상태를 변경하고, `App::view`는 현재 상태를 화면 요소로 변환한다. `main.rs`의 애플리케이션 연결과 창 설정은 다음과 같다. 설정 중 제목·렌더링 스타일 등은 생략하였다.

```rust
iced::application(boot, App::update, App::view)
    // executor 및 title 설정 생략
    .window(iced::window::Settings {
        size: Size::new(1360.0, 860.0),
        min_size: Some(Size::new(1080.0, 700.0)),
        ..Default::default()
    })
    // 글꼴·테마·스타일 및 중앙 배치 설정 생략
    .run()
```

GUI crate는 저장·변환 crate를 경로 의존성으로 선언하였다. 현재 화면의 가상 자료는 변환기를 거치지만, 저장 crate에 대한 의존성 선언 자체가 GUI의 SQLite 조회·저장 완료를 의미하지는 않는다. 데스크톱 중심의 세 영역 배치와 창 크기 제한을 유지하였으며, 좁은 창에서의 단일 영역 전환은 구현하지 않았다.

## UI 상태와 영구 저장

검토 파일: `crates/study-ui/src/main.rs`, `demo.rs`

GUI의 읽음·보관·검토 표시는 세 개의 `HashSet<usize>`로 관리한다. 현재 구현은 가상 자료 배열의 인덱스를 사용하며 SQLite에 해당 상태를 기록하지 않는다.

```rust
Message::Acknowledge => {
    if let Some(i) = self.selected {
        toggle(&mut self.acknowledged, i);
    }
}
```

위 분기는 검토 표시 집합만 변경하고 마감 데이터를 수정하지 않는다. 실제 저장소 연계 단계에서는 배열 인덱스에 의존하는 상태를 원본 식별자 및 레코드 ID와 연결하는 작업이 필요하다.

## 오류 처리

검토 파일: `crates/archive-store/src/error.rs`, `crates/catalog-import/src/lib.rs`

저장 계층은 입력 검증 실패, 잠금 충돌, 동일 시각의 내용 충돌 및 불완전 DB를 별도 오류 유형으로 구분한다. 초기화 실패 시에는 실패 단계와 정리 결과도 기록한다. 변환기는 전체 입력을 처리할 수 없는 오류와 개별 자료의 문제를 각각 `Result`와 `issues`로 반환한다.

모든 오류 유형을 실제 GUI에 표시하는 기능은 아직 연계하지 않았다.

## 검증 기록 {#검증-기록}

2026년 11월 7일 현재 소스에 대해 다음 명령을 실행하였다.

```bash
cargo test --offline --workspace -- --test-threads=1
```

| 모듈 | 통과 | 실패 |
| --- | ---: | ---: |
| archive-store | 26 | 0 |
| catalog-import | 11 | 0 |
| study-ui | 5 | 0 |
| 합계 | **42** | **0** |

주요 테스트와 검증 대상은 다음과 같다.

- `duplicate_order_does_not_change_result`: 중복 입력의 순서 독립성
- `body_seconds_conflict_and_fractional_instants_are_not_truncated`: 초·소수초 및 마감 근거 비교
- `close_reopen_split_catalog_retry_after_middle_batch_busy`: 분할 저장 중 잠금 오류와 재시도
- `review_acknowledgement_does_not_confirm_or_change_due_date`: 검토 표시와 마감 판정의 분리
- `read_saved_and_reset_are_memory_only_state`: 메모리 내 상태 및 초기화

검증에는 가상 자료와 임시 DB를 사용하였다. 실제 LMS 로그인·동기화, GUI의 영구 저장, 다른 OS에서의 실행 및 실제 저장 장치 고장은 검증 범위에 포함하지 않았다.

검토 근거는 `VALIDATION.md`, `PROTOTYPE_REVIEW.md`, `DATA_CONTRACT.md` 및 위 소스 파일이다. 포트폴리오 저장소의 [검토 목록](https://github.com/idiotot/portfolio/blob/main/docs/navi-implementation-review.md)에 파일별 SHA-256을 기록하였다.

[프로젝트 개요로 돌아가기](../)
