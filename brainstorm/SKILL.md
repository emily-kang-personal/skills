---
name: brainstorm
description: Generate several wildly different directions for a design or approach — cheapest to most ambitious — on one beautiful HTML page the user reacts to, so their Unknown Knowns (the taste and scope they can't articulate but recognize on sight) surface early instead of during expensive implementation. Use at the start of a session to set scope, or when the user says "brainstorm", "give me some directions", "I don't know what's possible", "react to some options", or "/brainstorm".
argument-hint: "[the rough problem, + any constraints or a target file/area]"
allowed-tools: Bash, Read, Grep, Glob, WebSearch, WebFetch, Skill
---

# Brainstorm

Attacks **Unknown Knowns**: criteria the user only knows how to define once they see it — visual taste, scope, "which approach feels right." Verbalizing these during a cheap brainstorm beats discovering them mid-build, where a small spec change can force a drastically different (and hard-to-revert) implementation.

## Procedure

1. **State the rough problem and constraints back**, then search the codebase for context so the directions are grounded in what's actually there.
2. **Generate several genuinely different directions** — default 4–6, ordered **cheapest → most ambitious**. Make them *wildly* different, not variations on one idea; the spread is what surfaces the user's hidden preference. Brainstorming guards against both too-narrow and too-wide a scope.
3. **Render them side by side** on one HTML page to react to. For visual/design work, show real mock tiles (fake data is fine). For approach/scope work, use comparison cards: what it is, cost, what it buys, what it rules out.
4. **Close with "tell me which resonate."** This is a reaction tool — the user picks, you don't decide for them.

## Output

One self-contained `.html` page written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** so directions read as distinct and polished; inline all CSS/JS; theme-aware and responsive.

## Don't

- Don't converge or recommend one — the user reacts; premature convergence defeats the purpose.
- Don't make the directions subtle variants of each other. Cheapest and most-ambitious should look nothing alike.
- Don't wire up anything real — this is pre-implementation.
