---
name: find-unknowns
description: Confront the four categories of unknowns (Known Knowns, Known Unknowns, Unknown Knowns, Unknown Unknowns) for a task, render them as a beautiful HTML quadrant map, and route each unknown to the cheapest technique that resolves it. The hub of the unknowns suite. Use when starting ambiguous or large work, working in an unfamiliar codebase/domain, or when the user says "find my unknowns", "what am I missing", "help me scope/de-risk this before I build", or "/find-unknowns".
argument-hint: "[the task or problem you're about to work on]"
allowed-tools: Bash, Read, Grep, Glob, WebSearch, WebFetch, Skill
---

# Find Unknowns

The map (your prompt, skills, context) is not the territory (the codebase, the real constraints). The gap between them is **unknowns**. This skill makes the gap visible and cheap to close *before* it gets expensive. Read `reference.md` for the four-quadrant model.

## Procedure

1. **Get the starting point first — this is the most load-bearing step.** Ask the user, in one short message: who they are on this task, their experience with this problem/codebase, and where they are in their thinking. Do not skip it — the same task has different unknowns for a novice and an expert.
2. **Research fast to fill the map.** Grep the codebase, read the relevant files, `git log` the history, WebSearch the domain, and use context7 for any library — Claude closes known gaps far faster than the user can.
3. **Classify the task into the four quadrants** (see `reference.md`). For **Unknown Knowns**, propose concrete candidates the user never wrote down and let them confirm — "you'd recognize it if you saw it." For **Unknown Unknowns**, teach: name what good looks like, prior art, and the potholes they haven't considered.
4. **Render one beautiful HTML quadrant map.** A 2×2 grid, every cell populated with specifics (never "TBD"). Tag each unknown with its **cheapest resolving technique** and a one-line why.
5. **Route.** Recommend the next skill(s) to run and say why, ordered cheapest-resolution-first: `/blindspot-pass` · `/brainstorm` · `/prototype` · `/interview` · `/reference` · `/implementation-plan`.

## Output

One self-contained `.html` file written to `./tmp/` (or the scratchpad if no `tmp/`), then `open` it. **Invoke the `frontend-design` skill for the visual craft** — distinctive, production-grade, theme-aware, responsive; inline all CSS/JS. The map is the deliverable, not a wall of prose.

## Don't

- Don't guess the user's experience level — ask (step 1).
- Don't leave a quadrant empty or vague. An empty Unknown Unknowns cell means you didn't research, not that there are none.
- Don't start implementing. This skill's job is to surface unknowns and route, nothing more.
