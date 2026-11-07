# NAVI Study 소스 검토 근거

2026-11-07, 로컬 프로젝트 소스를 읽고 포트폴리오 설명과 대조했다. 전체 프로젝트를 공개 저장소에 업로드하지 않았으며, 사이트 구현 노트에는 코드 일부와 상대 경로만 실었다.

## 직접 확인한 검증 결과

2026-11-07 20:13:28~20:13:32 KST, CachyOS Linux x86_64에서 `cargo test --offline --locked --workspace -- --test-threads=1`을 실행하였다. rustc 1.99.1 (58cf8f9c3 2026-10-13), cargo 1.99.1 (5f94df478 2026-08-27)을 사용하였으며 종료 코드는 0이다.

| 모듈 | 범위 | 통과 | 실패 |
| --- | --- | ---: | ---: |
| archive-store | 도메인 및 임시 파일시스템·SQLite 연계 | 26 | 0 |
| catalog-import | 파싱·변환 및 임시 저장소 연계 | 11 | 0 |
| study-ui | 창을 띄우지 않는 메모리 내 GUI 상태 처리 | 5 | 0 |
| 합계 | crate별 주 실행 결과 | 42 | 0 |

Cargo의 `unittests` 실행 안에 임시 DB 연계 검증도 포함되어 있다. 별도 child 프로세스의 보조 실행은 중복 합산하지 않았다. 문서 테스트는 0개이다. 가상 자료·임시 저장소를 사용하였으며 실제 LMS, GUI 영구 저장, 전체 화면 조작, 다른 OS 및 성능 측정은 포함하지 않았다.

- [정규화한 실행 로그](../site/static/evidence/navi-tests-20261107-round2.txt)
- [실행 환경·소스·로그 해시](../site/static/evidence/navi-tests-20261107-round2.json)

공개 로그에서는 절대 경로를 대체하였으며 인증 정보·실제 첨부파일·운영 DB를 포함하지 않았다. Rust·Cargo·SQL 파일과 Cargo.lock의 실행 전후 해시는 동일하였다. 테스트 fixture JSON은 실행 후 해시를 추가하였으며 이 구분을 manifest에도 기록하였다.

재현 명령은 원본 workspace에서 실행해야 한다. 동일한 소스·도구 버전·Cargo.lock 및 캐시된 의존성이 필요하다. 전체 앱 소스가 공개되지 않았으므로 이 포트폴리오 저장소만으로 독립 재현할 수는 없다.

## 검토한 원본 파일

아래 SHA-256은 검토 시점 파일의 해시다. 원본은 로컬 프로젝트이며 별도 공개 저장소 URL은 확인되지 않았다. 다음 목록은 원본 코드 공개 링크를 대신하는 전체 소스가 아니다.

| 파일 | SHA-256 |
| --- | --- |
| `Cargo.toml` | `38bab9c227d260f847f39b821e8eccc981ac4716c0cce1aefdccdbad74e31348` |
| `crates/archive-store/src/schema_v1.sql` | `92d89b853a81199fc1214cd152e3422f579fa57d4d7ee036b95f44716985d857` |
| `crates/archive-store/src/store.rs` | `7ad2af3317c21e40aa555daf525f256979ca75e06ddebf9ae1600dd6fbc307d7` |
| `crates/archive-store/src/domain.rs` | `44c847ae46157fac1a66af509dd06b79b854ae36b461f8391bbb7d602d2b30be` |
| `crates/archive-store/src/error.rs` | `8e427e27c0eae6d064e817954ba29c5d6f1cf9266b8849100c12bf1517ad9e68` |
| `crates/catalog-import/src/lib.rs` | `6e88c1fec494acf04a830faf5b9a3763863cb3ea0c663895d67aff054640bfd4` |
| `crates/catalog-import/src/evidence.rs` | `e7f288511a0db5fe4ba0587ba547fd08264a66d8870b80ea143b2f42a16055c5` |
| `crates/catalog-import/src/tests.rs` | `052494d0bcf083fb0b8b60bdc36409619b6dbe9f7f450efea303f76232439131` |
| `crates/study-ui/Cargo.toml` | `67614d4d3f7b2ce0d7c0879613a2d0d68bef4126e4eac4edf2e7f16034773eaa` |
| `crates/study-ui/src/main.rs` | `57c6bcaee08b26ed334f7dd9a09706f77c8bfd490cd4ec35314a01c3f30ebca0` |
| `crates/study-ui/src/demo.rs` | `f5dde576a331871ddec7b0b852d6401656e0bf5352cc81299e3baf278c665a8d` |
| `crates/study-ui/src/tests.rs` | `81194f1f102a624d5f41230bf829757ea359d344a29cb3921310486eef50b216` |
| `PROTOTYPE_REVIEW.md` | `8598339d70c20da4cc43fb27b6968b8c9f5d3f4ad53cee463465ab2de1f6f796` |
| `VALIDATION.md` | `c3c86ba444d1836cd72db92249a29398966cf692d035c0146fa989076ae2f077` |
| `DATA_CONTRACT.md` | `273e2c3ce30f1a6405ba9b5bfbc76d981d4c723c63046dc381cf4d052e0f2081` |

## 작성자가 제공한 선정 근거와 남은 확인 사항

Rust의 선정 근거는 작성자가 직접 제공했다. 런타임 성능, 컴파일 단계의 정적 검증, 메모리 안전성과 확장 가능성을 선택 기준으로 반영했다. 앱의 성능을 실측했다는 의미로 쓰지 않았다. 관련 언어 특성은 Rust와 Cargo 공식 문서로 확인했다.

2차 개선에서 작성자가 데스크톱 UI의 초기 기획과 Iced 선정 근거를 직접 제공하였다. 로컬 개인 사용에서 출발하여 범위 확장 후에도 데스크톱 콘셉트를 유지했다는 배경을 반영하였다. Iced는 Rust 통합성, 메시지 기반 상태 관리 및 GUI 구성·유지보수 관점에서 선정한 것으로 설명하였다.

`main.rs`의 `iced::application(boot, App::update, App::view)`, `Message`와 `App::update`, `view.rs`의 화면 구성 및 GUI crate의 경로 의존성이 해당 설명과 일치함을 확인하였다. 기본 1360×860·최소 1080×700 창 설정을 기록하고, 좁은 창 배치와 Windows·macOS 실행은 미구현·미검증 범위로 구분하였다. 비교 성능이나 모바일 대비 우위는 주장하지 않았다.

별도 앱 공개 저장소 URL은 아직 확인되지 않았다.
