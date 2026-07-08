---
name: quiet-tools
description: Cut wasted tool-call tokens — the invisible overhead of re-reads, verify-by-reread, sequential calls that could run in parallel, and narrating every tool use. Use for any multi-step task with several tool calls, or when the user says "stop re-reading", "don't narrate", "just do it", "빨리", "재확인 그만".
---

# quiet-tools — spend calls, not ceremony

Each tool call and its result costs tokens both ways. Most waste isn't one expensive call — it's many needless ones. Active every response until "stop quiet-tools" / "normal mode".

## The four leaks

1. **Verify-by-reread.** After Write/Edit, do not read the file back to "confirm." The tool errors if it fails; a success means it succeeded. The harness tracks the new state. Re-reading to admire your edit is pure overhead.
2. **Redundant reads.** Don't read the same file twice in a session. If you already have it (or a grep of it) in context, use that. If you need a different span, read only that span (see [[lean-context]]).
3. **Sequential when independent.** Two reads, two greps, three subagents with no data dependency → issue them in ONE message so they run in parallel. Waiting for A to finish before starting an unrelated B wastes a round-trip each time.
4. **Narration.** Don't announce "Now I'll read X", "Let me check Y", "I'm going to run Z" before each call — the call itself shows what you did. One line of intent before a *batch* is fine; a sentence before every call is tax.

## Rules

- Batch independent calls; chain only genuine dependencies.
- No "let me verify" reads of harness-tracked writes. Trust the success.
- Don't dump full tool output back to the user — surface the decisive line. (Big raw outputs → route through a script or subagent that returns the summary.)
- Prefer one precise call over three exploratory ones: think about *which* grep pattern / *which* line range before firing, not after.
- A failed/denied call is a signal — adjust, don't retry the same thing verbatim.

## When NOT to be quiet

Genuinely dependent steps must stay sequential (you need A's result to form B). Destructive or outward-facing actions still get their confirmation. Verification that runs *code* (tests, a build, a smoke check) is not "verify-by-reread" — that's real evidence, keep it. The rule kills redundant *reads*, not real checks.

Pairs with [[lean-context]] (read less) and [[no-restate]] (say less). This governs calls; those govern reading and prose.
