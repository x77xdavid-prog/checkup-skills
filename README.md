# checkup-skills

Claude Code용 스킬 플러그인 마켓플레이스입니다. 프롬프트 작성·한국 공공데이터 API 연동·repo 정리에서 실전 검증된 노하우를 스킬로 묶어 한 번에 설치할 수 있게 했습니다. [claude-checkup](https://github.com/x77xdavid-prog) 프로젝트의 일부로, "설치 장벽 없이 바로 써보는" 무료 스킬 모음입니다.

## 설치

```
/plugin marketplace add x77xdavid-prog/checkup-skills
/plugin install prompt-master@checkup-skills
```

두 번째 줄의 `prompt-master`를 아래 표의 다른 플러그인 이름으로 바꾸면 해당 스킬을 설치합니다.

## 스킬

| 플러그인 | 하는 일 | 트리거 예시 |
|---|---|---|
| **prompt-master** | 레퍼런스 사이트를 실측 분석해, 약한 모델이 실행해도 상위 모델급 결과가 나오는 완성 구현 프롬프트를 생성합니다. | "이 사이트처럼 만들 프롬프트", "3D 애니메이션 프롬프트" |
| **korea-public-data** | 한국 공공데이터 API(공공데이터포털·온비드·법원경매·HIRA·청약홈·실거래가) 연동의 인증키·응답·페이징·좌표계·캐싱 함정을 정리한 실전 패턴을 제공합니다. | "공공데이터 연동", "온비드", "실거래가" |
| **repo-shipper** | GitHub repo를 공개 기준(description·LICENSE·README·CI·.env.example·.gitignore)으로 감사하고 빠진 부분만 채웁니다. | "repo 정리", "README 만들어", "CI 붙여" |
| **pg-payments** | PG·카드결제 연동의 멱등성·웹훅 서명검증·정산 대사·PCI-DSS 범위를 다루는 결제 대행 실전 패턴을 제공합니다. | "결제 연동", "PG 어댑터", "빌링키", "정산 대사" |
| **three-3d-web** | Three.js·React Three Fiber로 3D/WebGL 웹을 만들 때의 프레임·바이트 예산, 모바일 폴백, reduced-motion을 정리한 성능 우선 패턴을 제공합니다. | "3D 히어로", "Three.js", "R3F", "GLTF 로딩" |
| **publisher-ad-revenue** | 애드센스·네이버 애드포스트 등 퍼블리셔 광고 수익을 RPM·뷰어빌리티·배치·정책 준수 관점에서 최적화합니다. | "애드센스 최적화", "RPM 올리기", "광고 배치" |
| **naver-blog-marketing** | 네이버 생태계(블로그·스마트스토어·플레이스) SEO를 C-Rank·D.I.A+ 기준으로 다루는, 구글 SEO와 다른 마케팅 패턴을 제공합니다. | "네이버 상위노출", "스마트스토어 SEO", "블로그 마케팅" |
| **stock-research** | 주식 데이터 수집·스크리너·백테스트를 다루는 리서치 패턴을 제공합니다. 룩어헤드·생존편향·과최적화 편향과 국내(KRX·DART)·미국 데이터 소스를 정리합니다. | "백테스트", "주가 데이터", "DART 재무제표" |
| **crypto-stablecoin-card** | USDT·USDC 스테이블코인 카드(Visa·Master), 온·오프램프, 카드 발급 스택, 커스터디, KYC/AML/특금법(VASP)을 컴플라이언스 우선으로 다룹니다. | "테더 카드", "스테이블코인 결제", "카드 발급", "온·오프램프" |
| **meta-ads-mcp** | 메타(페이스북·인스타) 광고를 Marketing API/MCP로 자동화하는 패턴을 제공합니다. (Meta MCP 또는 Marketing API 연결이 선행되어야 합니다.) | "메타 광고 자동화", "페이스북 광고 API", "CAPI 설정" |

## 내 클로드 점수 진단받기

claude-checkup 사이트에서 내 Claude Code 사용 점수를 진단받을 수 있습니다. (사이트 곧 공개 — 준비되면 여기에 링크가 올라갑니다.)

## 라이선스

MIT
