---
name: implementation-plan
description: Write an implementation plan as a beautiful HTML doc that LEADS with the decisions most likely to change — data model changes, new type interfaces, user-facing/UX flows — and buries the mechanical refactoring at the bottom, so the user reviews exactly the parts they might still want to alter. Grounds every claimed symbol/signature/path against real source before writing. Use when ready to implement and the user says "implementation plan", "plan this out", "write the plan", or "/implementation-plan".
argument-hint: "[the feature/change to plan, + the spec or prototype if you have one]"
allowed-tools: Bash, Read, Grep, Glob, WebSearch, WebFetch, Skill
---

# Implementation Plan

The point of the plan is to surface the things you might actually need to alter *before* they cost code. So the plan is ordered by likelihood-of-change, not by build order.

## Procedure

1. **Ground first.** Verify every symbol, signature, file path, and schema the plan will assert against the actual source — grep and read it. The plan's claims are a forecast; source wins. A bad line of a plan becomes thousands of bad lines of code.
2. **Lead with the decisions most likely to change:** data model changes, new type interfaces, and anything user-facing. These are what the user should spend review attention on.
3. **Bury the mechanical refactoring at the bottom** — the parts the user trusts you on. Keep them present but out of the way.
4. **Surface the top-of-doc decisions explicitly** for the user to tweak before anyone builds.

## Output

One self-contained `.html` plan written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for the render; inline all CSS/JS. Structure so the eye lands on the changeable decisions first and the mechanical work reads as an appendix.

## Don't

- Don't assert a code-fact from memory — verify it against source first (anti-drift).
- Don't front-load mechanical refactoring; that's exactly what the user doesn't need to review.
- Don't start implementing — this is the plan the user approves first.
