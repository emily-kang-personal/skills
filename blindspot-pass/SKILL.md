---
name: blindspot-pass
description: Teach the user their Unknown Unknowns in an unfamiliar domain or corner of the codebase — what questions they don't know to ask, what "good" looks like, prior art, and the potholes to avoid — as a beautiful HTML explainer that ends with sharper prompts they can now give. Use when starting work in a new part of the codebase or an unfamiliar subject, or when the user says "blindspot pass", "unknown unknowns", "what don't I know about X", "teach me so I can prompt better", or "/blindspot-pass".
argument-hint: "[the domain or codebase area you're new to, + what you already know]"
allowed-tools: Bash, Read, Grep, Glob, WebSearch, WebFetch, Skill
---

# Blind Spot Pass

Attacks **Unknown Unknowns**: what the user hasn't considered at all, including whether they even know how good the result can be. Claude knows more about the average topic than the user and searches faster — use that to teach.

## Procedure

1. **Calibrate to the user's starting point.** Get who they are and what they already know — this sets the depth. Teaching an expert the basics is noise; teaching a novice the frontier is noise. If they didn't say, ask in one line.
2. **Research hard.** Grep the codebase and `git log` for historical work already done here; WebSearch the domain for how it's actually practiced; context7 for any library. This is where the unknown unknowns live — don't answer from memory.
3. **Teach the blind spots.** Cover: the questions they don't know to ask, what "good" looks like (with a sense of the ceiling), prior/historical work, and the concrete potholes to avoid.
4. **Hand back sharper prompts.** End with a set of prompts they could now give — the point is to leave them able to prompt better, not just informed.

## Output

One self-contained `.html` explainer written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for distinctive, production-grade, theme-aware craft; inline all CSS/JS. Structure it to be *read*: sections, a "what good looks like" showcase, a potholes list, and a closing "prompts you can now give" block.

## Don't

- Don't teach generically — ground every claim in the actual codebase/domain research, not training-data recall.
- Don't skip the closing sharper-prompts block; enabling better prompting is the deliverable.
