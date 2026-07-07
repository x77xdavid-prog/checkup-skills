---
name: korea-public-data
description: Use when integrating Korean public/government data APIs — 공공데이터포털(data.go.kr), 온비드(공매), 법원경매, LH 분양·임대, HIRA 병원정보, 청약홈(applyhome), DART 전자공시, 국토부 실거래가. Covers 인증키 함정, XML/JSON, rate limit, 페이징, 좌표계 변환, SQLite 캐싱+cron 갱신. Triggers: 공공데이터, 온비드, 경매 데이터, HIRA, 청약홈, 실거래가, data.go.kr, 공매, LH.
---

# 한국 공공데이터 API 연동 패턴

여러 실전 프로젝트에서 검증된 한국 공공데이터 API 연동 패턴 모음.

## 인증키 — 최다 실패 지점

- data.go.kr 키는 **URL-인코딩 키 / 디코딩 키 2종**. requests/fetch가 자동 인코딩하므로 **디코딩 키를 파라미터로** 주거나, 인코딩 키를 쿼리스트링에 직접 박아라. 이중 인코딩 = `SERVICE_KEY_IS_NOT_REGISTERED_ERROR`.
- 키는 `.env`로만. 절대 커밋 금지. `.env.example`에 자리만.
- 신청 직후 키는 **1~2시간 활성화 지연**. 30 에러면 기다려라.

## 응답 처리

- 기본 XML. 대부분 `&_type=json` 또는 `dataType=JSON` 지원 — 항상 JSON로 받아라.
- 성공 판정은 HTTP 200이 아니라 **body의 `resultCode`**: `00`=정상, `03`=데이터없음(에러 아님), `22`=일일 트래픽 초과, `30`=키 미등록, `31`=기한 만료.
- 페이징: `numOfRows`(최대 보통 100~1000) + `pageNo`. `totalCount`로 루프 종료 판정.
- 개발계정 기본 **1,000회/일**. 초과 필요 시 운영계정 전환 신청(활용사례 작성 필요).

## 주요 API 실전 노트

| 소스 | 핵심 |
|---|---|
| 온비드 공매 | `OnbidPblsalThingInquireSvc` — 물건 목록·상세 분리 호출. 감정가 `apslAmt`, 최저입찰 `minBidPrc` |
| 법원경매 | **공식 API 없음** — 대법원 courtauction.go.kr 스크래핑. POST 폼 기반, 세션 쿠키 필요. 차단 시 우회 접근 패턴 적용 |
| 국토부 실거래가 | `RTMSDataSvc*` — `LAWD_CD` 법정동 **5자리**(시군구) + `DEAL_YMD` 월단위(YYYYMM). 건별 조회 불가, 월 통짜로 받아 필터 |
| HIRA 병원정보 | `hospInfoServicev2` — `ykiho`(암호화 요양기호)가 PK. 진료과목은 별도 엔드포인트 조인 |
| 청약홈 | `ApplyhomeInfoDetailSvc` — 분양공고 목록. 마감일 지난 것 필터 필수 |
| LH | `lhLeaseNoticeInfo1` — 임대공고. 페이징 파라미터가 `PG_SZ`/`PAGE`로 비표준 |
| DART | dart-fss(py) 또는 REST. 기업 고유번호 `corp_code`는 zip 다운로드 후 로컬 매핑 |

## 좌표계

공공데이터 좌표는 EPSG:5174/5181(중부원점) 혼재 — 지도에 찍기 전 **WGS84(EPSG:4326) 변환** 필수. JS `proj4`, Python `pyproj`. 변환 안 하면 핀이 바다에 뜬다.

## 캐싱·갱신 아키텍처 (기본값)

```
일배치 cron → API 수집 → SQLite upsert → 웹은 SQLite만 조회
```
- API를 요청마다 때리지 마라 — 일일 한도 소진 + 느림.
- upsert 키 = 공고번호/물건번호 같은 소스 PK. 수집시각 `fetched_at` 컬럼 필수(신선도 표시).
- 실패 시 이전 데이터 유지 + 로그. 빈 응답으로 테이블 덮어쓰기 금지 (fail-safe).

## 함정 선제 차단

- `03`(데이터없음)을 에러로 처리해 배치 전체를 죽이지 마라.
- XML 응답에 BOM/HTML 에러페이지가 섞여 올 수 있음 — 파싱 전 `<?xml`/`{` 시작 확인.
- 날짜 필드 포맷 제각각(YYYYMMDD, YYYY-MM-DD, 타임스탬프) — 소스별 파서 고정.
- 기관 점검시간(보통 새벽)에 5xx — cron은 오전 6시 이후로.
