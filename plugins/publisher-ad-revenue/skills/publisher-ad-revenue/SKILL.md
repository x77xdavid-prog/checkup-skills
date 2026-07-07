---
name: publisher-ad-revenue
description: Use when optimizing PUBLISHER / display ad revenue — earning money from ads on your own site/blog. Google AdSense, Naver 애드포스트, ad placement, RPM/eCPM/CTR/viewability, Auto ads, ads.txt, invalid-traffic/policy compliance, AdSense 승인. NOT for buying/running ads as an advertiser (that is the `ads` skill).
---

# Publisher Ad Revenue (애드센스 / 애드포스트)

## Overview
This is the **publisher side** — you own the content and earn from ads shown on it (opposite of the `ads` skill, which spends money buying ads). Revenue = **Traffic × RPM**, and RPM is driven by niche value, placement, viewability, and not getting policy-banned. The fastest way to zero income is an AdSense policy strike, so compliance is not optional.

## When to Use
- Setting up / optimizing Google AdSense or Naver 애드포스트
- Improving RPM/eCPM, fill rate, CTR, viewability
- AdSense 승인(approval) prep, policy/invalid-traffic issues, ads.txt
- Deciding ad placement without wrecking Core Web Vitals

## The revenue equation
```
Revenue = Pageviews × Page RPM
Page RPM = (Estimated earnings / Pageviews) × 1000
```
Levers, in order of impact:
1. **Niche/topic value** — finance, insurance, legal, B2B pay 10–50× more per click than entertainment. Content topic sets your ceiling.
2. **Traffic quality & geo** — US/KR high-intent > generic global.
3. **Placement & viewability** — above-fold, in-content, sticky units.
4. **CTR** — better placement, not deceptive clicks (deception = ban).

## AdSense vs 네이버 애드포스트
| | AdSense | 애드포스트 |
|--|---------|-----------|
| Platform | Any site/블로그 | 네이버 블로그·포스트 |
| Payout | $ CPC/CPM, high value | 원, 낮은 단가 |
| Approval | Strict content review | 네이버 계정 조건 |
| Best for | 자체 도메인, 영어권 트래픽 | 네이버 생태계 트래픽 |
Run both if you have both traffic sources; see `naver-blog-marketing` for driving 네이버 traffic.

## Placement that earns without hurting UX
- One strong **in-content** unit after the first section (highest viewability).
- **Above-the-fold** unit visible without scroll, but never pushing content down (CLS!).
- Sticky/anchor unit on mobile (allowed, high RPM) — don't stack many.
- Reserve ad slot height (min-height) to avoid layout shift → protects Core Web Vitals → protects rankings → protects traffic.

## Policy compliance (this is where income dies)
- **Never** click your own ads or ask others to ("클릭 유도" = 무효 트래픽 = 정지).
- No ads on thin/scraped/copyrighted/prohibited content.
- Keep ads clearly distinguishable from content (no "click here" arrows into ads).
- Maintain **ads.txt** at domain root listing authorized sellers.
- Watch invalid-traffic reports; bot/incentivized traffic gets accounts banned.

## Common Mistakes
- Chasing CTR with deceptive placement → policy strike, account termination.
- Too many ad units → viewability drops, RPM falls, UX + CWV tank.
- Ignoring content niche → high traffic, tiny RPM; a smaller finance site out-earns a huge meme site.
- Ads causing layout shift → CLS penalty → lost SEO traffic → lost revenue.
- No ads.txt → "unauthorized" sellers, revenue loss.
- Buying traffic to inflate views → invalid traffic ban.
