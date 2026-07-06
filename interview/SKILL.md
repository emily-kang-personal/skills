---
name: interview
description: Interview the user one question at a time about the ambiguities in a task — prioritizing the questions whose answer would change the architecture — and capture the resolved decisions into a beautiful HTML spec. Attacks Known Unknowns: the gaps they can name but haven't decided. Use after brainstorming when ambiguity remains, or when the user says "interview me", "ask me questions about this", "pin down the spec", "what's still ambiguous", or "/interview".
argument-hint: "[the task or draft spec you want interrogated]"
allowed-tools: Bash, Read, Grep, Glob, Skill
---

# Interview

Attacks **Known Unknowns**: the decisions the user is aware they haven't made. Pulls them out efficiently by asking, not guessing.

## Procedure

1. **Get context to aim the questions** — the problem, any draft spec, where the user is in their thinking. Grep the codebase so questions are specific, not generic.
2. **Ask ONE question at a time and wait for the answer.** Never batch. **Prioritize questions whose answer would change the architecture** — data model, seams between components, what type carries what, the lifecycle the app must drive. Burn the high-leverage ambiguities first; a session that runs out of the user's patience should have spent it on decisions that matter.
3. **Log each answer into a running spec as you go**, so nothing decided gets re-litigated.
4. **When the load-bearing ambiguity is resolved, render the final HTML spec** — the accumulated decisions, clean.

## Output

One self-contained `.html` spec written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for the final render; inline all CSS/JS. During the interview itself, just ask in chat — the HTML is the closing artifact.

## Don't

- Don't ask more than one question per turn. The one-at-a-time cadence is the technique.
- Don't lead with cosmetic questions — architecture-changing answers come first.
- Don't invent answers the user didn't give; unresolved items stay marked open in the spec.
