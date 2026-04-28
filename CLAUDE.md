# Aside-skill · Working Context

This file helps any AI assistant (including future Claude Code sessions) onboard quickly into this project. It is not user-facing documentation.

## What this project is

Aside is a cross-platform Agent Skill that names cognitive patterns mid-conversation. It cites public psychology / behavioral economics sources, refuses to diagnose, and stays silent outside the "腹地" (mid-conversation) position.

**v0.1 is Chinese-first.** English version in v0.1.1, Japanese in v0.1.2.

## Key files

- `README.md` — user-facing (Chinese)
- `SKILL.md` — core instruction file (English directive + Chinese output voice)
- `DECISIONS.md` — 8 key decisions from the 2026-04-20/21 cabinet meeting (read this for "why we did it this way")
- `docs/PRD.md` — full product specification
- `references/decision-rationalization.md` — 13 L1 entries (decision rationalization)
- `references/conversation-patterns.md` — 2 L2 entries (conversation patterns)
- `templates/output.md` — exact output block format
- `tests/golden-set.md` — 25 behavioral test cases (L1 + L2)

## Five core design principles

1. **Name + cite + aside-tone** — never diagnose, never advise, never explain causes
2. **Mid-conversation principle (腹地原则)** — no conversation openings, no endings, no crises
3. **Conservative over spammy** — max 1 trigger per 15 exchanges; user pushback → stop entirely
4. **Silence beats distortion** — if the block can't render cleanly, stay silent
5. **Parasitic on host semantic judgment (寄生宿主)** — no hard detection rules, trust the host AI's language understanding

## Output voice rules

- Opens with **"顺便一提——"** (conversational connector, never a structural header)
- Uses **"很像 XXX 这个现象"** — observational, not declarative ("this is X" is banned)
- Includes **"不一定完全对得上"** — epistemic humility built in
- Closes with **"也许值得回头看一眼。"** — invitational, never prescriptive

## Current status

- v0.1 scaffold: complete
- Entries in `references/` are sketches — definitions and signatures need refinement
- Golden set has 25 cases — needs manual runthrough on 4 host clients before v0.1 ships
- See `docs/PRD.md` § 12 for the open Backlog

## When working in this repo — do NOT

- Add MBTI, Big Five, or personality tests (anti-Aside philosophy)
- Add CBT cognitive distortions (clinical territory, out of scope)
- Hard-code trigger rules (violates principle 5 — use semantic guidance to the host AI)
- Write philosophical musings in README (user-facing stays pragmatic)
- Use high-posture "who this is for" filtering language (describe positive fit, don't push people away)

## Technical choices

- **Form**: Pure Skill (not MCP) in v0.1 — covers Claude Code, Claude Desktop, Claude.ai, Codex, OpenClaw, Trae, CodeBuddy via the skills.sh ecosystem
- **MCP version**: deferred to v0.2, only if Cursor/Windsurf users explicitly need it
- **Inspired by**: alchaincyf/nuwa-skill and alchaincyf/darwin-skill patterns

## Quick map of the 5 Chinese terms kept from iteration

- **腹地原则 (Mid-conversation principle)** — Aside only works in the middle of a conversation
- **顺便一提 voice** — the product's entire speaking style, from user's own observation
- **寄生宿主 (Parasitic on host)** — the product architecturally trusts the host AI's semantics
- **吧/对吧 signal** — Chinese linguistic marker for confirmation-seeking (user insight)
- **装之前先想一下** — README positioning tone (soft invitation, not filtering)

These five concepts together define Aside's identity. Don't dilute them.
