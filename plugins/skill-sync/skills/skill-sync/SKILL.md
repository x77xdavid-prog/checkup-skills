---
name: skill-sync
description: Update installed Claude Code skills to their latest upstream version, and discover new skills worth adding. Use when the user says "update my skills", "check for skill updates", "get the latest skills", "내 스킬 최신으로", "스킬 업데이트", "새 스킬 있어?", or wants to keep their ~/.claude/skills and plugin marketplaces current.
---

# skill-sync — keep skills current

Skills evolve upstream; installed copies drift stale. This skill checks what you have against its source and updates it — safely, with the user in the loop for anything destructive.

## 1. Inventory what's installed

- Plugin marketplaces: list `~/.claude/plugins/marketplaces/*/` — each is a git clone. `git -C <dir> remote get-url origin` gives the upstream.
- Loose skills: `~/.claude/skills/*/SKILL.md` (and `~/.claude/commands/*.md`). These may or may not have a known source.

## 2. Check each for updates

- **Marketplace clones (easy):** `git -C <dir> fetch` then compare `git rev-list HEAD..@{u} --count`. >0 = updates available. Show the incoming commit subjects so the user sees what changes.
- **Loose skills with a git remote:** same fetch/compare.
- **Loose skills without a source:** if a `provenance.json`-style map exists (source repo per skill), use it. Otherwise report "source unknown — can't auto-update" honestly; don't guess a repo.

## 3. Update (with consent)

- Marketplace: `git -C <dir> pull --ff-only` (fast-forward only — never force, never merge-conflict silently). If it can't fast-forward, stop and report; the user may have local edits.
- Then remind: plugin skills need a Claude Code restart / `/reload-plugins` to take effect.
- Loose skill from a repo: re-fetch its SKILL.md and diff against the installed copy. **Show the diff, get an OK before overwriting** — the user may have customized it. Never blind-overwrite a file you didn't write.
- Batch: update all clean fast-forwardable clones in one pass, list the ones that need attention separately.

## 4. Discover new (optional)

If asked "any new skills?": check the marketplaces the user already trusts for plugins they haven't installed, and surface those. Don't add anything without asking. Only pull from marketplaces already in their config — never install from an arbitrary URL on your own.

## Safety rules

- `--ff-only` and diff-then-confirm are non-negotiable. Updating a skill is editing the user's tooling — treat it like any file change you didn't author: look before you overwrite.
- Never modify a skill that has uncommitted local changes without flagging it.
- A marketplace update is a `git pull` on a repo the user chose — safe. Installing a *new* marketplace from a URL is not — that needs explicit approval each time.
- Report: updated / up-to-date / needs-attention / source-unknown, as a short table. Then the restart reminder if anything changed.

Pairs with the checkup catalog's provenance data (source repo per skill) when available.
