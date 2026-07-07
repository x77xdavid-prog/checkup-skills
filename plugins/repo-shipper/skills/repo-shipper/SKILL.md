---
name: repo-shipper
description: Use when polishing a GitHub repo to a public-ready standard — audit and fill the common defects (description, LICENSE, README, CI) plus .env.example and .gitignore. Triggers: repo 정리, README 만들어, LICENSE 추가, CI 붙여, 포트폴리오 정리, repo 완성도, 공통 결함.
---

# repo-shipper — repo 공통결함 메우기

기본 규칙: **모든 공개 repo에 description·LICENSE·CI·README**. 이 스킬은 그 감사와 채우기를 한 번에 한다.

## 0. 먼저 — 운영중이면 재배포 주의

push가 재배포를 유발하는 repo(GitHub Pages, Render, Vercel, Cloudflare Worker 등 CI/CD가 걸린 것)는 **파일 변경 전에 배포 영향을 먼저 확인**한다. README·LICENSE 추가 커밋도 재배포다. 순서: 재배포 없는 것(description·topics)부터 → 파일 변경은 영향 확인·승인 후. 실서비스 데이터가 있는 repo는 특히 주의.

## 1. 감사 (읽기만, 5분)

```bash
gh repo view {repo} --json description,licenseInfo,hasIssuesEnabled
ls README.md LICENSE .github/workflows/ .env.example .gitignore 2>/dev/null
```

체크: [ ] description(한 줄, 한국어+핵심키워드) [ ] LICENSE [ ] README(아래 표준) [ ] CI [ ] .env.example(시크릿 쓰는 repo만) [ ] .gitignore(node_modules/.env/dist)

## 2. 채우기

**description** (재배포 없음 — 항상 안전):
```bash
gh repo edit {owner}/{repo} --description "한 줄 설명" --add-topic korea --add-topic fintech
```

**LICENSE**: MIT 기본. 저작권자는 the repository owner.

**README 표준 구조** (한국어, 과장 금지, 실측만):
```markdown
# {이름} — {한 줄}
{2-3문장: 누가 왜 쓰나. 실사용 중이면 명시}
## 스크린샷 (웹/앱이면 필수 1장)
## 실행
{설치→환경변수→실행, 복붙 가능하게}
## 스택
## 라이선스
```

**CI** (`.github/workflows/ci.yml`) — 언어별 최소:
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # node: setup-node@v4 → npm ci → npm test --if-present → npm run build --if-present
      # python: setup-python@v5 → pip install -r requirements.txt → pytest || python -m compileall .
```
테스트 없는 repo는 빌드/컴파일 체크만이라도 — 빨간불 CI보다 초록 최소 CI.

**.env.example**: 실제 값 금지, 키 이름+설명 주석만.

## 3. 검증

- CI는 push 후 `gh run watch`로 초록 확인까지가 완료. 빨간불 방치 금지.
- README의 실행 명령은 **직접 한 번 실행해보고** 적는다. 안 돌아가는 README는 없느니만 못함.

## 함정

- 이미 있는 README를 통짜 재작성하지 마라 — 빠진 섹션만 추가.
- LICENSE 추가는 커밋이다 = 운영 repo면 재배포. §0 참조.
- private repo 백업 목적이면 CI 생략 가능 (비용/노이즈).
