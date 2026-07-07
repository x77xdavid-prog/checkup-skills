---
name: meta-ads-mcp
description: Use when automating Meta (Facebook / Instagram) advertising programmatically — via an MCP server or the Meta Marketing API. Campaign / ad set / ad creation, custom & lookalike audiences, Conversions API (CAPI), insights & ROAS reporting, automated rules. Covers 메타 광고 자동화, 페이스북/인스타 광고 API. Note: requires a Meta Marketing API app or Meta MCP to be connected first.
---

# Meta Ads Automation (MCP / Marketing API)

## Overview
Meta advertising automates through the **Marketing API**, structured as a strict 3-level hierarchy: **Campaign → Ad Set → Ad**. An MCP server is just a wrapper that exposes these API calls as tools. The two things that make or break automated Meta advertising today: correct **Conversions API (CAPI)** setup (iOS 14.5+ killed pixel-only tracking) and not tripping automated policy review.

## Connection prerequisite (check first)
Meta has **no first-party MCP** in the registry. The de-facto standard is pipeboard's
**remote MCP** — no local install, one URL: `https://meta-ads.mcp.pipeboard.co/`.
Prereq: a [pipeboard.co](https://pipeboard.co) account with your Facebook Ads account
linked (this is where the actual Meta OAuth happens). Then connect one of two ways:

**A) claude.ai (Pro/Max, browser):** claude.ai → Settings → Integrations → Add,
name "Pipeboard Meta Ads", URL `https://meta-ads.mcp.pipeboard.co/`, follow OAuth.

**B) Claude Code CLI:**
```
claude mcp add --transport http meta-ads https://meta-ads.mcp.pipeboard.co/
```
Then authenticate in an interactive session with `/mcp` (OAuth), **or** skip OAuth by
appending a token from pipeboard.co/api-tokens:
```
claude mcp add --transport http meta-ads "https://meta-ads.mcp.pipeboard.co/?token=YOUR_PIPEBOARD_TOKEN"
```
OAuth/token auth cannot complete in a non-interactive session — if not connected,
say so and design the automation only.

Once connected, have your Ad account ID (`act_XXXX`), Pixel/Dataset ID, and Page ID ready.

## API hierarchy
```
Campaign      → objective + budget (CBO), e.g. OUTCOME_SALES
  └ Ad Set    → audience, placement, budget, schedule, optimization goal
      └ Ad    → creative (image/video/carousel) + copy + destination
```
Create top-down; each child references its parent's ID.

## Audiences
- **Custom Audience** — from your data (customer list hashed, site visitors via pixel, engagers).
- **Lookalike (유사 타겟)** — 1–10% similarity seeded from a custom audience.
- **Broad + Advantage+** — let Meta's ML target; increasingly the recommended default.

## Conversions API (CAPI) — do not skip
Browser pixel alone under-reports since iOS 14.5 (ATT). Send conversions **server-side** via CAPI, deduplicated with the pixel using a shared `event_id`. Without CAPI, optimization and ROAS reporting are blind. This is the single highest-leverage setup step.

## Reporting / ROAS
- Pull via the **Insights API**: spend, impressions, CTR, CPA, ROAS, by campaign/adset/ad.
- Automate rules (pause ad set if CPA > X, scale if ROAS > Y) via Automated Rules or your own scheduled job.

## Common Mistakes
- Pixel-only tracking (no CAPI) → 20–40% under-reported conversions, mis-optimization.
- Editing an ad set's audience/optimization mid-learning → resets the learning phase.
- Too many tiny ad sets → each never exits learning phase (needs ~50 conversions/week).
- Missing `event_id` dedup → CAPI + pixel double-count conversions.
- Storing the system-user token in code → secret manager; long-lived tokens are dangerous.
- Assuming the MCP is connected → verify the connector/API access before running calls.
- Non-compliant creative (before/after, personal-attribute targeting) → auto-rejected/account flagged.
