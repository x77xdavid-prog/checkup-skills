---
name: lean-context
description: Cut input/context tokens — the biggest sink in agentic coding. Locate before you read, read only the lines you need, and delegate large reads to a subagent that returns a digest. Use when a task will touch big files, long logs, wide searches, or an unfamiliar codebase, or when the user says "save tokens", "context is full", "don't read the whole file", "토큰 아껴", "컨텍스트 꽉 참".
---

# lean-context — read less into context

Output tokens are what you write; **input tokens are what you pour in by reading.** In a real coding session the input side is usually larger and quieter. This skill trims it. Active every response until "stop lean-context" / "normal mode".

## The ladder (stop at the first rung that answers the question)

1. **Do you need the file at all?** The answer may already be in the conversation, a prior tool result, or the error message. Don't re-read to "make sure."
2. **Can a locator answer it?** `grep`/glob returns the line and its number — often that IS the answer (does X exist, where is Y defined). Search before you open.
3. **Do you need the whole file?** Read the specific range (`offset`+`limit`) around the grep hit, not 1–2000. Config value, one function, one import — read that, not the module.
4. **Is it big or repetitive?** (large file, long log, wide multi-file sweep) → delegate to a subagent and ask for a **digest**, not the raw content. The subagent burns its own context; you get the 5-line conclusion.
5. **Only then:** read the minimum span that actually answers the task.

## Rules

- Never re-read a file you just wrote or edited — the harness already tracks its current state. Editing would have errored if it failed.
- Long logs / stack traces: quote the shortest decisive line, don't paste the dump. Grep the log for the error, read ±10 lines.
- Broad "how does this work" questions → dispatch an Explore/search subagent that reads excerpts and returns the map. Keep the file dumps out of your context, keep the conclusion.
- Big JSON/CSV/data files → process with a script (`python`/`node`) that prints only the aggregate, don't read rows into context.
- Pass paths, not contents: when handing work to a subagent, give the path and let it read — don't read it yourself first and paste it.

## When NOT to trim

Security-sensitive review, a bug whose cause you can't localize, or anything where a missed line changes the answer: read fully. lean-context shortens *incidental* reading, never the reading the task depends on. When unsure whether a span matters, read it.

Pairs with [[quiet-tools]] (fewer calls), [[token-budget]] (a target to trim toward). Governs what you read, not how you write — combine with caveman for output.
