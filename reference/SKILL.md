---
name: reference
description: When the user can't describe what they want in words, point Claude at a real reference — a repo folder, a file, a library, or a live site's underlying code — read the actual source (not a screenshot), and reimplement the same semantics in the target stack, even across languages. Produces a beautiful HTML comparison of the reference's behavior vs the planned implementation. Use when the user says "make it work like this", "copy the behavior of X", "reimplement this crate/component in our stack", "here's a reference", or "/reference".
argument-hint: "[path/URL/library to use as the reference, + what to copy from it]"
allowed-tools: Bash, Read, Grep, Glob, WebSearch, WebFetch, Skill
---

# Reference

Sometimes the user lacks the language for what they want, or it'd take too long to describe. The best answer is a reference — and the best reference is **source code**, not a picture.

## Procedure

1. **Get the reference and what to copy from it** — a repo folder, a specific file, a library, or a live site. Ask what behavior/semantics matter (backoff timing? layout structure? state machine?).
2. **Read the actual source.** For code, read it directly even if it's a different language than the target. For a website, read the underlying markup/structure/how the component is built — not just the rendered screenshot. This is where the richer detail lives.
3. **Reimplement the same semantics in the target stack.** Preserve the behavior that matters; adapt idioms to the destination language/framework.
4. **Render an HTML comparison**: the reference's behavior vs your implementation plan — what carries over exactly, what changes because the stack differs, and any semantics you deliberately dropped.

## Output

One self-contained `.html` comparison written to `./tmp/` (or the scratchpad), then `open` it. **Invoke the `frontend-design` skill** for the render; inline all CSS/JS. Show reference and target side by side.

## Don't

- Don't reason from a screenshot when the source is available — read the real markup/code.
- Don't copy line-by-line across languages; copy the *semantics*, written idiomatically for the target.
- Don't silently drop a behavior — if a semantic doesn't carry over, name it in the comparison.
