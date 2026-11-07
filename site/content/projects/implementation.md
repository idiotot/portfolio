+++
title = 'NAVI Study 구현 노트'
url = '/projects/lms/implementation/'
description = '검토한 NAVI Study 소스의 저장 구조, 중복·마감 처리, UI 상태와 테스트 기록입니다.'
+++

2026년 11월 7일 로컬 프로젝트의 소스와 검증 문서를 확인한 내용입니다. 경로는 NAVI Study 프로젝트 루트를 기준으로 합니다. 아래 코드는 해당 파일의 일부를 발췌했습니다.

## 저장 구조 {#저장-구조}

`crates/archive-store/src/schema_v1.sql`의 `records`는 `(kind, source_key)`를 기본키로 사용합니다. 자료의 제목과 JSON payload, 현재 자료의 존재 상태와 관측 시각을 저장합니다.

```sql
PRIMARY KEY(kind,source_key)
```

`watermarks`는 `(course_id, kind)`마다 마지막 관측 시각과 완전한 목록 처리 여부를 기록합니다.

```sql
CREATE TABLE watermarks (
    course_id TEXT NOT NULL,
    kind TEXT NOT NULL CHECK(kind IN ('course','document','attachment','event')),
    observed_at INTEGER NOT NULL CHECK(observed_at > 0),
    complete INTEGER NOT NULL CHECK(complete IN (0,1)),
    PRIMARY KEY(course_id,kind)
) STRICT;
```

`store.rs`의 쓰기 처리는 레코드와 watermark를 하나의 트랜잭션에서 갱신합니다. 같은 관측 시각의 동일 레코드 재시도는 허용하고, 같은 시각에 내용이 다른 레코드는 충돌로 처리합니다. 이 스키마에는 GUI의 읽음·보관 표시를 저장하는 필드가 아직 없습니다.

## 자료 중복 처리

검토 파일: `crates/catalog-import/src/lib.rs`, `source.rs`

변환기는 과목 ID·원본 종류·원본 ID로 만든 키로 문서를 묶습니다. 같은 키의 문서가 여러 개면 `DuplicateMerged` 문제 유형을 기록하고, 최신 `fetched_at`을 선택합니다. 같은 시각에는 경로와 제목으로 순서를 정하므로 입력 배열의 순서가 달라도 결과가 같습니다.

게시판의 글 ID는 게시판 ID와 함께 보존합니다. 서로 다른 과목이나 원본의 자료를 파일 해시가 같다는 이유로 합치지는 않습니다.

## 마감일 판정

검토 파일: `crates/archive-store/src/domain.rs`, `crates/catalog-import/src/evidence.rs`

마감 판정에는 다음 세 상태를 사용합니다. 선언에서 직렬화 속성은 생략했습니다.

```rust
pub enum Deadline {
    Unknown,
    Confirmed { value: DeadlineCandidate },
    Conflict { candidates: Vec<DeadlineCandidate> },
}
```

각 후보는 시각과 `Evidence { path, line, quote }`를 가집니다. 완전한 날짜·시각은 시차를 반영한 같은 순간인지 비교하고, 후보가 하나면 확정합니다. 서로 다른 완전한 시각이 남으면 충돌입니다. 날짜의 일부만 있는 본문과 공식 날짜가 맞지 않는 경우에는 확인 필요 상태로 남깁니다.

## UI 상태와 영구 저장

검토 파일: `crates/study-ui/src/main.rs`, `demo.rs`

GUI의 읽음·보관·검토 표시는 `HashSet<usize>` 세 개로 관리합니다. 현재는 가상 자료 배열의 인덱스를 사용하며 SQLite에 쓰지 않습니다.

```rust
Message::Acknowledge => {
    if let Some(i) = self.selected {
        toggle(&mut self.acknowledged, i);
    }
}
```

이 분기는 검토 표시 집합만 바꾸고 마감 데이터를 바꾸지 않습니다. 실제 저장소와 연결할 때는 배열 인덱스 대신 원본 식별자와 레코드 ID를 사용하는 작업이 남아 있습니다.

## 오류 처리

검토 파일: `crates/archive-store/src/error.rs`, `crates/catalog-import/src/lib.rs`

저장 계층은 입력 검증 실패, 잠금 충돌, 같은 시각의 내용 충돌, 불완전 DB 등 오류를 구분합니다. 초기화 실패에는 실패 단계와 정리 결과도 남깁니다. 변환기는 전체 입력을 처리할 수 없는 오류와 개별 자료의 문제를 구분해 `Result`와 `issues`로 반환합니다.

이 오류들을 실제 GUI에서 모두 표시하는 기능까지 연결한 것은 아닙니다.

## 검증 기록 {#검증-기록}

2026년 11월 7일 현재 소스에서 아래 명령을 실행했습니다.

```bash
cargo test --offline --workspace -- --test-threads=1
```

| 모듈 | 통과 | 실패 |
| --- | ---: | ---: |
| archive-store | 26 | 0 |
| catalog-import | 11 | 0 |
| study-ui | 5 | 0 |
| 합계 | **42** | **0** |

주요 테스트는 다음과 같습니다.

- `duplicate_order_does_not_change_result`: 중복 문서의 입력 순서와 결과
- `body_seconds_conflict_and_fractional_instants_are_not_truncated`: 초·소수초와 마감 근거 비교
- `close_reopen_split_catalog_retry_after_middle_batch_busy`: 분할 저장 중 잠금 오류와 재시도
- `review_acknowledgement_does_not_confirm_or_change_due_date`: 검토 표시와 마감 판정 분리
- `read_saved_and_reset_are_memory_only_state`: 메모리 내 상태와 초기화

이 검사는 가상 자료와 임시 DB를 사용합니다. 실제 LMS 로그인·동기화, GUI의 영구 저장, 다른 OS 실행과 실제 저장 장치 고장을 검증한 결과는 아닙니다.

검토 근거: 프로젝트의 `VALIDATION.md`, `PROTOTYPE_REVIEW.md`, `DATA_CONTRACT.md`와 위 소스 파일. 포트폴리오 저장소의 [검토 목록](https://github.com/idiotot/portfolio/blob/main/docs/navi-implementation-review.md)에 파일별 SHA-256을 기록했습니다.

[NAVI Study 프로젝트로 돌아가기](../)
