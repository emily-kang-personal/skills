# The four categories of unknowns

From Thariq's "A Field Guide to Fable: Finding Your Unknowns." When Claude hits an unknown, it guesses what you want. The more work, the more unknowns. Reducing and planning for them is *the* skill of agentic coding.

| Quadrant | Definition | How to tell | Cheapest technique |
|---|---|---|---|
| **Known Knowns** | What you've told the agent you want. Essentially your prompt. | You can write it down plainly. | Just state it in the prompt. |
| **Known Unknowns** | Gaps you're aware of but haven't figured out. | You can name the question but not the answer. | `/interview` — answer them one at a time. |
| **Unknown Knowns** | So obvious you'd never write it down, but you'd recognize it if you saw it. Often taste, scope, visual feel. | You keep saying "I'll know it when I see it." | `/brainstorm` + `/prototype` — react to something concrete. |
| **Unknown Unknowns** | What you haven't considered at all. Knowledge you're not aware of. Whether you even know how good it can be. | You don't know what questions to ask. | `/blindspot-pass` — have Claude teach you the domain. |

## Two rules that make this work

1. **Context about your starting point beats everything.** Tell Claude who you are, your experience with the problem and codebase, and where you are in your thinking. The identical task has different unknowns for different people — without your starting point, Claude fills the gap with generic industry best-practice that may not fit.
2. **Every explainer, brainstorm, interview, prototype, and reference is a cheap way to find out what you didn't know before it gets expensive to fix.** Unknowns found in a prototype cost minutes; the same unknown found deep in implementation can force an expensive revert. Surface early.

## The instruction balance

- **Too specific** → Claude follows you off a cliff even when a pivot was right.
- **Too vague** → Claude guesses from industry defaults that may not fit your task.
- Accounting for your unknowns is how you avoid failing both ways: you want Claude to veer on the obstacles you can't see, and hold the line where you actually have intent.
