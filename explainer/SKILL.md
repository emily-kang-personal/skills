---
name: explainer
description: Package a finished change — prototype, spec, implementation notes, and the diff — into one beautiful, shareable HTML doc that gets buy-in, leading with the demo and accounting for the unknowns and failure modes an expert reviewer would ask about. Written for a reviewer who starts with the same unknowns the user did. Use post-implementation when the user says "package this for buy-in", "make an explainer", "write it up for Slack/the team", "pitch this change", or "/explainer".
argument-hint: "[the change to package, + any prototype/spec/notes artifacts]"
allowed-tools: Bash, Read, Grep, Glob, Skill
---

# Explainer

Shipping isn't done until you have buy-in. A good explainer accelerates understanding for reviewers who start with the same unknowns you did, and accelerates approval by showing experts you accounted for the failure points they'd anticipate.

## Procedure

1. **Gather the artifacts:** the prototype, the spec, the implementation notes (especially deviations), and the actual diff. Read the diff so the write-up is grounded, not aspirational.
2. **Lead with the demo** — a GIF, screenshot, or the prototype embedded up top. Reviewers buy the outcome before the mechanism.
3. **Then the substance:** the problem, the unknowns you resolved and how, the approach, and the failure modes you anticipated (this is what earns expert approval — showing you saw the potholes).
4. **Package into one shareable doc** the user can drop into Slack/a PR and have a reviewer understand cold.

## Output

One self-contained `.html` doc written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** so it's persuasive and polished — this is customer/reviewer-facing; inline all CSS/JS; theme-aware and responsive.

## Don't

- Don't bury the demo — lead with it.
- Don't invent results or gloss the failure modes; accounting for them honestly is what gets the approval (and every business/technical claim must trace to the real diff/artifacts).
- Don't dump the raw diff as the explainer — translate it into what changed and why it's safe.
