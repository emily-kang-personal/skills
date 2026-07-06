---
name: quiz
description: After a long working session, produce a beautiful HTML report on what changed — context, intuition, what was done and why, including the behavior that depends on existing code paths a diff alone won't show — with a quiz at the bottom the user must pass before merging. Closes the user's own post-build Unknown Unknowns about their own change. Use when the user says "quiz me on this change", "help me understand what happened", "I want to pass a quiz before I merge", or "/quiz".
argument-hint: "[the branch/diff/change to be quizzed on]"
allowed-tools: Bash, Read, Grep, Glob, Skill
---

# Quiz

After a big session Claude may have done more than the user realized, and reading the diff only gives a light understanding — much of the behavior lives in existing code paths the diff doesn't show. This skill builds real understanding and gates the merge on it.

## Procedure

1. **Read the diff AND the code paths it depends on.** Grep for the callers, the state it touches, the existing behavior it plugs into — the parts a diff alone hides. This context is what the quiz will test.
2. **Write the report:** what changed, the context around it, the intuition for *why* it works, and how it interacts with existing code.
3. **Add a quiz at the bottom** — questions that genuinely require understanding the change and its interactions, not surface recall.
4. **Grade the user's answers.** State plainly: merge only after a perfect pass. If they miss one, that miss is a real unknown — explain it.

## Output

One self-contained `.html` report-plus-quiz written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for the render; inline all CSS/JS. Report on top, quiz at the bottom.

## Don't

- Don't quiz only on the diff — the interactions with existing code paths are the point.
- Don't write surface-recall questions; test understanding that would catch a wrong mental model.
- Don't wave the user through a partial pass — a missed question is an unknown to close before merge.
