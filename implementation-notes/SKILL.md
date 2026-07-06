---
name: implementation-notes
description: While implementing, keep a running implementation-notes.md logging every place the code forced a deviation from the plan — pick the conservative option, record what/why/alternatives under "Deviations", and keep going without stopping to ask on reversible calls — then render the log as a beautiful HTML report at the end. Captures the Unknown Unknowns that only surface mid-build. Use when starting to implement a plan, or when the user says "keep implementation notes", "log deviations as you go", or "/implementation-notes".
argument-hint: "[the plan/spec being implemented]"
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, Skill
---

# Implementation Notes

No amount of planning removes every unknown — the agent finds edge cases in the code that force a different tack mid-build. This skill captures those so the next attempt learns from them.

## Procedure

1. **Create `implementation-notes.md` in the working directory** at the start of the build.
2. **On every forced deviation from the plan:** pick the **conservative** option, log it under a `## Deviations` section (what the plan said · what the code forced · why · alternatives considered), and **keep going**. Do not stop to ask on a two-way-door (reversible) decision — momentum is the point.
3. **Keep an `## Open Questions` section** for anything genuinely needing the user; surface the whole list at the very end, not mid-flight.
4. **At the end, render the notes as a beautiful HTML report** so the deviations and open questions are reviewable at a glance.

## Output

Working log: `implementation-notes.md` in the repo (plain markdown — it's a live scratchpad during the build). Final: one self-contained `.html` report written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for the final render; inline all CSS/JS.

## Don't

- Don't stop mid-build to ask about a reversible call — log it and move on.
- Don't silently deviate — an unlogged deviation is a lost lesson.
- When forced to choose without a clear signal, choose the conservative option and note it.
