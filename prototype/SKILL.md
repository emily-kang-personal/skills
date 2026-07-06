---
name: prototype
description: Mock the ONE thing the user needs to react to — a layout, a toolbar, an interaction — as a single self-contained HTML file with fake data, before wiring up any backend, route, or real state. Surfaces Unknown Knowns (layout/feel the user recognizes on sight) at minutes of cost instead of an expensive mid-build revert. Use when the user says "prototype", "mock this up", "let me see the layout before you build it", "fake data is fine", or "/prototype".
argument-hint: "[the UI/interaction to mock, + where it lives in the app]"
allowed-tools: Bash, Read, Grep, Glob, Skill
---

# Prototype

Attacks **Unknown Knowns** by making them concrete to react to. You want to see how a button on a frame looks *without* wiring a backend route or maintaining new frontend state.

## Procedure

1. **Pin down the ONE thing to react to** — the layout, the toolbar, the interaction flow. A prototype that tries to show everything shows nothing.
2. **Build a single self-contained HTML file with fake data.** No backend, no real routes, no new state in the real app. Hardcode representative data so the shape and feel are honest.
3. **`open` it** and say plainly: "React to this before I touch the real app."

## Output

One self-contained `.html` file written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** so the mock reflects real production quality, not a greybox — the user is reacting to feel, so feel has to be there. Inline all CSS/JS.

## Don't

- **Do not touch the real app.** No new routes, no state wiring, no backend. The whole value is that this is throwaway and cheap.
- Don't broaden past the one thing being reacted to.
- Don't use lorem-ipsum where real-shaped fake data would reveal a layout problem (long names, empty states, big numbers).
