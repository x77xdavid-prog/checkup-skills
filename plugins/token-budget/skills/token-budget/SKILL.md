---
name: token-budget
description: Put a token target on a task and work to it — pick depth to fit the budget, report rough spend, and escalate only within it. Use when the user sets a limit ("keep it under ~50k", "quick pass", "don't spend all day", "cheap version", "싸게", "토큰 예산") or when a task is open-ended and could balloon (broad research, whole-repo audit, "find all the bugs").
---

# token-budget — a target to spend against

Open-ended tasks ("research X", "audit the repo", "find all bugs") have no natural stopping point, so an agent keeps going until it drifts or runs out. A budget turns that into a decision: **how much depth does this much token buy?** Active for the current task until met or "stop token-budget".

## Set the budget

- If the user gave a target ("~50k", "quick", "cheap", "thorough"), use it. Map words to size: *quick/cheap* = shallow single pass; *thorough/deep* = multi-pass with verification.
- If not, propose one in a line and proceed: "Scoping this as a ~30k quick pass — one sweep, top findings only. Say 'go deep' for more." Don't stall waiting for confirmation on a default.

## Work to it

- **Pick depth up front, not midway.** A small budget = one pass, best-effort, name what you skipped. A large budget = decompose, parallelize, verify. Don't start deep and get cut off with nothing finished.
- **Scale the fleet to the budget.** Few subagents for a small target, many for a large one. Loop-until-dry only when the budget affords the tail.
- **Report spend in rough terms** at checkpoints: "~half the budget, covered A and B, C remains — continue or stop here?" Give the user the stop decision before you blow past it.
- **Escalate inside the ceiling.** Ran a cheap heuristic and it's ambiguous? Spend more *only if budget remains*. When it's gone, deliver what you have plus an honest "what a bigger budget would add."

## No silent overrun

- A budget is a ceiling, not a suggestion. Approaching it → surface it, don't quietly double it.
- If the task genuinely can't be done within the target, say so at the start with a realistic number — don't half-do it and imply it's complete.
- Name every coverage cut a budget forces (sampled not exhaustive, top-N only, no verify pass) so "done within budget" never reads as "covered everything."

## When NOT to cap

Correctness-critical work (security, money, data-loss paths, a bug the user is blocked on) is done right, not to a number. Budget scopes *breadth and depth of effort*, never the rigor of a check that must be correct.

Pairs with [[lean-context]]/[[quiet-tools]] (spend less per unit of work, so the budget goes further) and [[token-diet]] (which of these to run).
