# agent-kit

Personal kit for agent skills and Cursor rules.

## Layout

```
skills/engineering/   daily code-work skills
skills/productivity/  workflow skills
rules/                Cursor rules (source; link into a project or ~/.cursor/rules)
```


## Skills

### Engineering

| Skill | What it does |
| --- | --- |
| `diff-cleanup` | Strip unneeded code from a change, leave the rest maintainable, then run tests/lint/format until green. |
| `refactor-scope` | Mark a named refactor's blast radius with searchable `REFACTOR:` comments (user-invoked; does not implement). |
| `summarize-diff` | Explain a branch or working-tree diff in simple language: what changed and why it matters. |
| `suggest-commits` | Propose commit messages and staging groups from the current diff, matched to the repo's history style. |
| `prototype` | Build throwaway logic or UI prototypes to answer a design question. |

### Productivity

| Skill | What it does |
| --- | --- |
| `grill-me` | User-invoked entry point that starts a grilling session. |
| `grilling` | Relentless interview that stress-tests a plan or design until nothing is left assumed. |

`prototype` is a fork of [Matt Pocock's prototype skill](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md). See [ATTRIBUTION.md](ATTRIBUTION.md).

These copies are source only. They are not installed over `~/.agents/skills`.
