---
name: refactor-scope
description: >-
  Mark the blast radius of a named refactor with REFACTOR comments on
  in-scope files and parts (functions) so the scope shows in git diff and
  is searchable. Invoke when scoping a refactor, not when implementing it.
  Markers are scaffolding; the human later passes the finished change to
  diff-cleanup.
disable-model-invocation: true
---

# Refactor Scope

Materialize the **scope** of a named refactor as `REFACTOR:` comments on the files and parts within them that must change together. The markers are scaffolding so the human can see the blast radius in git diff, search for `REFACTOR:`, and refactor in place. Stop after marking. Do not implement the refactor.

**Passoff:** after the human finishes the refactor, they run **diff-cleanup**, which strips leftover `REFACTOR:` markers with the rest of the scaffold.

## 1. Name the goal

Take the refactor goal from the user (rename, extract, move a boundary, etc.). If it is missing, ask once and wait.

Completion: one-sentence goal is stated.

## 2. Map the blast radius

Explore for every file and every part within those files that must change with that goal: definition sites, callers, tests, config, types, adjacent modules that move together. Prefer evidence from the codebase over guesswork.

A **part** is a function, method, class, type, or other coherent section that belongs in the refactor. Whole-file scope is fine when the file moves as a unit; otherwise mark only the parts that do.

Completion: the in-scope path set is listed, with the parts inside each file, including any no-comment formats that cannot be marked.

## 3. Insert markers

For each in-scope file that supports comments, insert `REFACTOR:` markers:

- **File**: one file-level marker, as early as practical after shebang / license header / file-level docblock / frontmatter, stating why the file is in scope.
- **Parts**: one marker immediately above each in-scope part (function, method, class, type, section), stating what that part does and why it is included in the refactor.

Delimiter by language:

| Languages | Form |
| --- | --- |
| Python, shell, YAML, TOML, Ruby, Perl, Make | `# REFACTOR: <description>` |
| JS, TS, Go, Rust, Java, Kotlin, C, C++, Swift, etc. | `// REFACTOR: <description>` |
| HTML, XML, Vue/Svelte markup | `<!-- REFACTOR: <description> -->` |
| JSON and other no-comment formats | Skip; report only |

`<description>` is: what this section does, and why it is included in the refactor. Examples:

```text
# REFACTOR: constructs UserService; callers must switch to the extracted factory
# REFACTOR: validates session tokens; moves into AuthGate with the login path
```

Hard rules:

- Mark only in-scope files and parts — skip unrelated neighbors in the same file
- Marker text states *what this section does* and *why it belongs*, not a restatement of the global goal alone
- Do not start implementing the refactor
- Leave the markers in place for the human; diff-cleanup removes them on passoff

Completion: every commentable in-scope file and part has a marker; no-comment paths are only in the report.

## 4. Report

```text
## Scope

Goal: <one sentence>

Marked:
- <path>: <why the file>
  - <part>: <why the part>
- <path>: <why the file>

Skipped (no comments):
- <path>: <why it is still in scope>

Next: refactor in the marked files, then pass off with diff-cleanup
```

Completion: report delivered; work stops.
