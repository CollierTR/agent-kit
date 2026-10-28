---
name: suggest-commits
description: >-
  Suggests commit messages and staging groups from the current diff,
  matched to the style of the repo's commit history. A singular commit when the
  remainder is one unit; grouped commits otherwise. Use when the user
  asks for commit messages, how to group files to stage, or whether
  everything should be one commit.
---

# Suggest Commits

Read the repo's **history**, read the **remainder** (the current uncommitted diff), then propose a **plan**: a **singular** commit when the change is one unit, or grouped commits with the files to stage for each. Stop after the plan; leave staging and committing to the user.

## 1. Read the history

Pull enough recent commits to see the house style (`git log --oneline` on this branch, and on the upstream if the branch is short). If the repo documents a commit convention, that document wins over inferred history.

State the style you will match: subject shape, body habit, ticket references, tense.

Completion: those style constraints are stated.

## 2. Read the remainder

Gather staged, unstaged, and untracked changes (`git status`, `git diff HEAD`, untracked file contents). Treat the current index as a hint, not the plan. If the working tree is empty, say so and stop.

Read the diffs. Name the behaviors, not just the file list.

Completion: every changed path is in context, each with a one-line note of what it does in this remainder.

## 3. Decide singular vs grouped

Prefer a **singular** commit when every path serves one why — one behavior, one fix, one refactor — and a split would leave a commit that does not stand on its own.

**Group** into more than one commit when the remainder holds distinct whys (a feature plus unrelated cleanup, two independent fixes, a refactor that can land before the behavior change). Each group must be independently sensible: the tree after that commit should build and mean something.

Mixed hunks in one file can belong to different groups; say so when a file has to be split by hunk or cannot be split cleanly.

Keep secrets and credential files out of every group, and name them if they appear in the remainder.

Completion: every path in the remainder is assigned to exactly one group, or called out as a hunk-split / leave-unstaged exception.

## 4. Write the messages

For each group, write a message that matches history's shape. Prefer why over what when history does not forbid it. Subject first; body only when history uses bodies or the why does not fit the subject.

Completion: each group has a message that a reader of `git log --oneline` could tell apart from the others.

## 5. Report the plan

Singular:

```
## Commit plan

Singular — <the one why>

Message:
<subject>

Stage:
- <path>
```

Grouped:

```
## Commit plan

N commits — <why they split>

### 1. <subject>
Stage:
- <path>

Message:
<full message>

### 2. ...
```

Give the groups in the order they should be committed. If a file cannot be split cleanly across groups, say which commit should take the whole file and why.
