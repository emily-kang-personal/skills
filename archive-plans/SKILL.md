---
name: archive-plans
description: Snapshot the project's ENTIRE tmp/ tree into the projects-history repo — every file and subdir (briefs, ready-plans, future-plans, cancelled-plans, done-plans, reviews, loose files, dotfiles), preserving structure — plus sweeping reviews/ from the main checkout AND all live worktrees. Copies rather than moves — tmp/ stays as local scratch. Each run is an idempotent snapshot; identical files are no-ops, changed files commit as updates. The ONLY thing it asks is which phase folder to land in; everything else proceeds automatically. Use when asked to "archive plans", "snapshot tmp", "save plans to history", or "/archive-plans". Run before pruning worktrees — archiving is what makes worktree-local review artifacts survive.
---

# archive-plans

Snapshot the current project's **entire `tmp/` tree** into `~/Documents/Projects/projects-history/<repo>/<phase>/`. The project repo's `tmp/` stays untouched (local scratch per project convention — see `feedback_never_commit_tmp`). The projects-history repo gets a remote-linkable, version-controlled mirror that downstream tools like `/link-plan-PR` can reference.

**Archive everything.** This skill mirrors *every* file under `tmp/` — known plan/brief subdirs, `reviews/`, loose files (e.g. `codex-deferred.md`), and dotfiles/dirs (e.g. `.codex-reviews/`), preserving the relative directory structure. There is no allowlist. The only two special cases are routing rules (where a thing lands), not exclusions:

- **`reviews/retros/`** mirrors REPO-LEVEL (`<repo>/retros/`), not into the phase folder — retros are a continuous weekly series, not phase-scoped.
- **`reviews/`** is *additionally* swept from every live worktree (not just the main checkout).

Everything else under `tmp/` lands phase-nested, structure preserved.

> `tmp/` is local scratch and may contain secrets or large/binary scratch files. "Archive everything" is deliberate (Emily's call) — projects-history is a private repo. The skill does not scrub; the user owns what's in `tmp/`.

## tmp/ structure (the conventional subdirs, for reference)

```
<repo>/tmp/
  briefs/             — exploratory docs, audits, scoping notes
  ready-plans/        — plans actively in progress or pending implementation
  future-plans/       — plans scoped for a later phase; not yet active
  cancelled-plans/    — plans explicitly cancelled
  done-plans/         — plans shipped
  reviews/            — per-run /review-timebox artifacts (phase-nested; ALSO swept from worktrees)
  reviews/retros/     — weekly saturday-retro decision logs (REPO-LEVEL placement)
  <anything else>     — loose files, dotfiles, other subdirs — ALL archived, structure preserved
```

The whole tree (minus the `reviews/retros/` re-route) mirrors **phase-nested**:

```
~/Documents/Projects/projects-history/<repo>/<phase>/
  briefs/  ready-plans/  future-plans/  cancelled-plans/  done-plans/  reviews/
  <plus every other file/subdir found under tmp/, at the same relative path>
```

**`reviews/` worktree sweep** (decision 2026-06-04: review artifacts stay worktree-local — a worktree process must NEVER write into the main checkout's tree; the archiver is the persistence mechanism instead):

- Enumerate worktrees with `git worktree list --porcelain` from the repo root.
- For each worktree (including the main checkout), scan `<worktree>/tmp/reviews/*.md` (NOT retros/ — retros only live in the main checkout).
- Merge all sources into one `reviews/` destination set. Byte-identical same-named files dedupe as no-ops; same-named files with DIFFERENT content are a conflict — list both source paths and ask which wins (filenames carry branch+SHA markers, so real collisions should be rare).
- This sweep is why /archive-plans must run **before** pruning worktrees.

**`reviews/retros/` → `<repo>/retros/`** (repo-level):

```
~/Documents/Projects/projects-history/<repo>/retros/
  <YYYY-MM-DD>-retro.md
```

## Companion explainer HTMLs (planned vs. actual)

An `/explain-diff` walkthrough of a plan's shipped diff is written to
`/tmp/<date>-explanation-<slug>.html` — **outside** the repo, per that skill's own
convention — so it is invisible to this skill's normal `<repo>/tmp/` sweep. Before
archiving a plan that has a companion explainer, copy it into the **same directory as
its plan** (typically `tmp/done-plans/`) as `<plan-filename>-explainer.html`. The
`-explainer` suffix is required: a plan may already have its own rendered
`<plan-filename>.html` from `/plan` tooling, and the two must never collide (check
first — do not overwrite a plan's own HTML). Once copied into `<repo>/tmp/...`, it's
just another file this skill's normal sweep picks up and mirrors phase-nested, no
special-casing needed past that point.

This pairs the plan (**planned** — the forecast, decisions, and rationale written
*before* implementation) with the explainer (**actual** — background/intuition/code-
walkthrough/quiz written and verified against the real, shipped diff, possibly
including post-review follow-up fixes the plan never anticipated). `/link-plan-PR`
posts a link to both when both exist for the same plan — see that skill's step 8.

## Workflow

### 1. Locate sources

- `git rev-parse --show-toplevel` → repo root.
- Check `/tmp/*-explanation-*.html` for any companion explainer whose slug matches a
  plan being archived this run; copy matches into the plan's directory as described
  above, before scanning `tmp/`.
- Scan the **entire** `<repo-root>/tmp/` tree recursively (every file, including dotfiles and nested subdirs). Split out `tmp/reviews/retros/**` for repo-level routing.
- Enumerate live worktrees (`git worktree list --porcelain`) and additionally scan each worktree's `tmp/reviews/*.md`.
- Build the file list, preserving each file's path relative to `tmp/` (reviews/ entries note which worktree they came from).
- If `tmp/` is missing or empty, stop and tell the user there's nothing to archive.

### 2. Determine the destination phase folder (the ONE question this skill asks)

- Repo name = `gh repo view --json name -q .name` (fall back to basename of the toplevel; NOTE the toplevel basename may be a worktree codename, so prefer `gh`).
- Look at `~/Documents/Projects/projects-history/<repo>/` for existing phase folders.
- Pick the phase folder using this order of preference:
  1. **Exactly one phase folder exists** → use it.
  2. **Today's date fits inside the date range encoded in a phase folder name** (e.g., `worker-substrate-05-11-to-05-18-2026`) → use that folder.
  3. **Today's date is past the end of every existing phase folder** → propose a new phase folder. Default name pattern: `<short-slug>-<MM-DD>-to-<MM-DD>-<YYYY>`. Ask the user to confirm/rename, OR offer an existing thematically-matching folder to extend.
  4. **Ambiguous** → list candidates with their date ranges and ask.

**This is the only thing to ask the user.** Present the phase-folder choice (existing-vs-new + name), get the answer, and then proceed automatically — do NOT additionally gate on a per-file confirmation. A one-line summary of what will be archived (counts) is fine to show, but the only *question* is the folder.

If a new phase folder will be created, `mkdir -p` only after the user confirms the name.

**Retros bypass phase selection entirely** — `tmp/reviews/retros/*` always mirrors to `<repo>/retros/`.

### 3. Pre-flight diff scan (internal — drives no-op detection, not a gate)

Classify each source file for the report and the empty-commit check:

- `[new]` — destination doesn't exist.
- `[overwrite — content differs]` — destination exists, content differs.
- `[unchanged]` — destination byte-identical (no-op; git won't stage it).

Use `cmp -s <source> <destination>`. This is for reporting and the no-op short-circuit — it is NOT a confirmation gate.

### 4. Copy files + commit + push in projects-history

1. `mkdir -p` each destination directory needed (preserve the tree under `<phase>/`, plus `<repo>/retros/` for retros).
2. `cp <source> <destination>` for every file (overwrite by default).
3. `cd ~/Documents/Projects/projects-history`.
4. `git add <specific paths>` for each copied file (specific paths only; never `git add .` / `git add -A`). Git skips identical files automatically.
5. If `git diff --cached --quiet` (nothing staged) → report "nothing to commit; `tmp/` is identical to the last snapshot" and stop.
6. Otherwise commit:

   ```
   chore: snapshot tmp/ from <repo> (<phase>)

   Source: <repo>@<short-sha-of-HEAD-in-source-repo>
   Files (grouped by relative dir):
     briefs/:
       - <file>
     reviews/:
       - <file>
     <other dirs / loose files>:
       - <file>
   ```

7. `git push`.
8. Verify with `git status` (local up-to-date with `@{u}`).
9. Save the commit SHA + GitHub URL for the report.

### 5. Report

```
Snapshotted N file(s) from tmp/ (entire tree).

projects-history (pushed):
  Commit: <sha>
  URL:    https://github.com/emilykangdev/projects-history/commit/<full-sha>
  Path:   <repo>/<phase>/   (+ <repo>/retros/ if retros were swept)
  Dirs touched: <dir> (N), <dir> (N), ...

<repo>: untouched (tmp/ is local scratch).
```

## Hard rules

- **Ask only about the phase folder.** That's the single confirmation. Don't add a per-file approval gate.
- **Archive everything under `tmp/`** — no allowlist, structure preserved. The only re-route is `reviews/retros/` → repo-level.
- **Copy, never move.** The source repo's `tmp/` is local scratch — never modified (no `git rm`, no `mv`, no edits). It stays gitignored per `feedback_never_commit_tmp`.
- **Specific paths only** when calling `git add`. Never `git add .` / `git add -A`.
- **Empty-snapshot is a no-op.** If nothing changed since the last run, report and exit without committing.
- **Push projects-history immediately** after committing — downstream tools (e.g., `/link-plan-PR`) need the remote files to exist.
- **One commit per archival run.** Group everything in a single commit body.
- **Never push the source repo from this skill.** It doesn't touch the source repo at all.
