# NAVI Study 소스 검토 근거

2026-11-07, 로컬 프로젝트 소스를 읽고 포트폴리오 설명과 대조했다. 전체 프로젝트를 공개 저장소에 업로드하지 않았으며, 사이트 구현 노트에는 코드 일부와 상대 경로만 실었다.

## 직접 확인한 검증 결과

명령: `cargo test --offline --workspace -- --test-threads=1`

결과 요약: archive-store 26개, catalog-import 11개, study-ui 5개, 합계 42개 통과 / 실패 0개. 별도 child 프로세스의 보조 실행은 중복 합산하지 않았다. 가상 자료·임시 DB를 사용했다. 이 기록은 이번 실행의 요약이며 전체 터미널 로그의 원문은 아니다.

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

## 본인 확인이 필요한 정보

Rust와 Iced를 처음 선택한 개인적 계기는 소스만으로 확인할 수 없다. 사이트에는 확인한 기술 구성과 현재 구현 범위를 설명하고, 개인적인 선택 이유를 임의로 작성하지 않았다.
