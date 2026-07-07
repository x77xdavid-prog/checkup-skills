# checkup-skills

Claude Code용 스킬 플러그인 마켓플레이스입니다. 프롬프트 작성·한국 공공데이터 API 연동·repo 정리에서 실전 검증된 노하우를 스킬로 묶어 한 번에 설치할 수 있게 했습니다. [claude-checkup](https://github.com/x77xdavid-prog) 프로젝트의 일부로, "설치 장벽 없이 바로 써보는" 무료 스킬 모음입니다.

## 설치

```
/plugin marketplace add x77xdavid-prog/checkup-skills
/plugin install prompt-master@checkup-skills
```

두 번째 줄의 `prompt-master`를 `korea-public-data` 또는 `repo-shipper`로 바꾸면 해당 스킬을 설치합니다.

## 스킬

| 플러그인 | 하는 일 | 트리거 예시 |
|---|---|---|
| **prompt-master** | 레퍼런스 사이트를 실측 분석해, 약한 모델이 실행해도 상위 모델급 결과가 나오는 완성 구현 프롬프트를 생성합니다. | "이 사이트처럼 만들 프롬프트", "3D 애니메이션 프롬프트" |
| **korea-public-data** | 한국 공공데이터 API(공공데이터포털·온비드·법원경매·HIRA·청약홈·실거래가) 연동의 인증키·응답·페이징·좌표계·캐싱 함정을 정리한 실전 패턴을 제공합니다. | "공공데이터 연동", "온비드", "실거래가" |
| **repo-shipper** | GitHub repo를 공개 기준(description·LICENSE·README·CI·.env.example·.gitignore)으로 감사하고 빠진 부분만 채웁니다. | "repo 정리", "README 만들어", "CI 붙여" |

## 내 클로드 점수 진단받기

claude-checkup 사이트에서 내 Claude Code 사용 점수를 진단받을 수 있습니다. (사이트 곧 공개 — 준비되면 여기에 링크가 올라갑니다.)

## 라이선스

MIT
