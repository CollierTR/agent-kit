---
name: diff-cleanup
description: >-
  Cleans a git diff: strips unneeded code, leaves the remainder maintainable
  and readable, then runs the project's tests, lint, and format suite. Use
  when the user asks to clean up a diff, strip leftover scaffolding from a
  change, or make a change review-ready.
---

# Diff Cleanup

Read the change, strip everything that does not earn its keep, leave the **remainder** maintainable and readable, then run the **suite** until it is green. Stop after the report; leave committing to the user.

## 1. Pin the diff

Resolve the range:

- User named a fixed point → `git diff <fixed-point>...HEAD`, plus any working-tree changes
- Working tree is dirty → the working tree against `HEAD`, including untracked files
- Working tree is clean → `git diff <main>...HEAD` (resolve `main` / `master` / the tracked upstream from git)

State the range. Gather the file list and the full diff. Work in source humans maintain; skip lockfiles, generated output, and vendor trees.

Completion: the range is stated, and the file list plus full diff are in context.

## 2. Find every unneeded site

Walk every hunk and every newly added file. Read enough surrounding code to tell leftover from load-bearing.

A line is **unneeded** when deleting it would not change the behavior the change exists to deliver:

- Dead code, unused imports, unused exports, unused parameters
- Debug leftovers (`console.log`, `debugger`, stray prints)
- Commented-out code
- Scaffold from the implementation (WIP notes, finished TODOs, always-true flags)
- Speculative generality: abstractions, params, or hooks the change does not use
- Duplicate logic that can collapse into one place
- Comments that restate the next line

Apply the **deletion test** to new wrappers and helpers: if deleting them only moves the same lines to the caller, they are unneeded. Scope is the change.

Completion: every unneeded site in the diff is listed, or you have confirmed there are none.

## 3. Clean the remainder

Edit only files in the change. Cut every listed site. When a test only asserted unneeded behavior, remove that test with the code.

What remains should be something a teammate can maintain:

- Names reveal what the thing does or holds
- Control flow is straight: early returns, one job per function
- The surrounding file's style is the style
- A new type or helper exists only when it hides real complexity

Preserve public behavior. If a cut would change what callers observe, keep the code.

Completion: every listed site is gone, and the remainder reads as intentional.

## 4. Run the suite

Discover the project's **suite** from the environment: task-runner scripts, Makefile, `justfile`, CI config. Use those commands.

Run, in order: format, lint, tests. A single `check` / `ci` / `verify` script counts as the whole suite when it covers those. Skip any piece the project does not define, and say so.

On failure caused by this cleanup: fix and re-run. On a pre-existing failure outside the change: note it and stop.

Completion: every defined piece of the suite is green, or every remaining failure is pre-existing and named.

## 5. Report

```
## Cleanup

Range: <the pinned range>

Removed:
- <file>: <what and why>

Rewrote:
- <file>: <what made it more readable>

Suite: format ✓  lint ✓  tests ✓
```

Omit empty sections. If nothing was unneeded, say so and still report the suite.
