---
name: summarize-diff
description: >-
  Summarizes a git diff in simple language: what changed and why it
  matters. Use when the user asks to summarize a diff, explain current
  changes, or describe what a branch or working tree changed.
---

# Summarize Diff

Read the change, then report a **simple-language** summary a teammate could follow without opening the diff. Stop after the report; leave editing and committing to the user.

## 1. Pin the diff

Resolve the range:

- User named a fixed point → `git diff <fixed-point>...HEAD`, plus any working-tree changes
- Working tree is dirty → the working tree against `HEAD`, including untracked files
- Working tree is clean → `git diff <main>...HEAD` (resolve `main` / `master` / the tracked upstream from git)

State the range. Gather the file list and the full diff. Work in source humans maintain; skip lockfiles, generated output, and vendor trees.

Completion: the range is stated, and the file list plus full diff are in context. If the range is empty, say so and stop.

## 2. Read the change

Walk every hunk and every newly added file. Read enough surrounding code to name behaviors, not just paths.

Completion: every changed path is understood well enough to explain what it does in this change.

## 3. Summarize in simple language

Explain what changed and why it matters for a reader who has not seen the diff.

- Prefer concrete outcomes over file names, APIs, or jargon
- Group related edits into areas
- Call out risk or surprising deletes when they are present

Completion: the whole change has a short overview, and each area has a plain-language note.

## 4. Report

```
## Diff summary

Range: <the pinned range>

<2–4 sentence plain-language overview of the whole change>

By area:
- <area or path group>: <what changed, in simple terms>
```

Omit empty sections. Do not invent commit messages or edit the tree.
