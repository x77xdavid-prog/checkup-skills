---
name: token-diet
description: The conductor for token reduction — decides which token-saving skill to run for the situation, since each targets a different sink. Use when the user broadly wants to spend fewer tokens / go cheaper / faster without a specific technique in mind ("save tokens", "make this cheaper", "reduce cost", "토큰 줄여줘", "싸게 해줘", "효율적으로"), or asks what the token-diet family does.
---

# token-diet — one diet, five levers

A Claude Code session leaks tokens in five distinct places. No single skill covers all of them — each lever targets a different sink. This skill picks the right one(s) instead of reaching for whichever you remember.

## The five sinks → the lever that plugs it

| Where tokens leak | Lever | One-line |
|---|---|---|
| **Reading in** (files, logs, wide searches) | [[lean-context]] | locate before read, read the span not the file, delegate big reads for a digest |
| **Tool-call overhead** (re-reads, verify-by-reread, serial calls) | [[quiet-tools]] | trust writes, batch parallel, no per-call narration |
| **Writing out** (prose padding) — light | [[no-restate]] | say each thing once, no preamble/postamble |
| **Writing out** (prose) — aggressive | caveman | terse fragments, drop articles, 65% output cut |
| **Code volume** (over-building) | ponytail | laziest solution that works, stdlib/native first |
| **Overall depth/breadth** (open-ended tasks) | [[token-budget]] | set a target, work to it, report spend |

## How to choose

- **"just save tokens" (no specifics)** → default combo: **lean-context + quiet-tools + no-restate**. These three cut input, calls, and prose padding with near-zero quality cost. Announce the combo in one line, then work.
- **Big reading/research task** → lead with lean-context (+ token-budget if open-ended).
- **Many tool calls / a build-heavy loop** → quiet-tools.
- **Replies feel padded** → no-restate; if they want it *terse* (fragments, telegraphic) → caveman instead.
- **Writing/refactoring code** → ponytail (fewer lines) — a different axis from the reading/output levers, safe to run alongside any of them.
- **"keep it cheap / under X / quick"** → token-budget wraps the rest.

## Rules

- These compose. lean-context (input) + caveman (output) + ponytail (code) target different axes and stack without conflict. Don't treat them as either/or.
- Never diet away correctness: input validation, security, error handling, data-loss guards, and anything explicitly requested stay full. Every lever in this family has the same carve-out — token saving is never an excuse to skip a real check.
- State which levers you turned on ("using lean-context + quiet-tools") so the user can steer. Off with "normal mode".

Family: [[lean-context]] · [[quiet-tools]] · [[no-restate]] · [[token-budget]] · caveman · ponytail.
