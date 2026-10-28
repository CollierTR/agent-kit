---
name: prototype
description: Build a throwaway prototype to answer a design question. Use when the user wants to sanity-check whether a state model or logic feels right, or explore what a UI should look like.
---

# Prototype

A prototype is **throwaway code that answers a question**. The question decides what you build.

## Pick a branch

Figure out which question you're answering from the user's prompt, the surrounding code, or by asking if they're around:

- **"Does this logic / state model feel right?"** → [LOGIC.md](LOGIC.md). One shareable HTML file with free-play buttons and tabbed walkthroughs. Push the awkward cases a non-developer can click through.
- **"What should this look like?"** → [UI.md](UI.md). A floating button on the app shell opens a throwaway comparison page with every variant stacked and labeled. Tap one, hit back, and try it on the real chrome.

These two builds look nothing alike, so the wrong branch wastes the prototype. If you're not sure and the user isn't around, guess from the surrounding code (a backend module → logic; a page or component → UI) and say so at the top of the prototype.

## Rules that apply to both

1. **Throwaway from day one, and named that way.** Put it next to the module or page it's for, but name it so nobody mistakes it for production. For throwaway UI routes, follow the project's existing routing — don't invent a new top-level structure.
2. **Trivial to run.** A UI prototype starts from one command in the project's task runner: `pnpm <name>`, `python <path>`, `bun <path>`, etc. A logic demo is a single HTML file they double-click. Either way, no setup to think about.
3. **No persistence by default.** Keep state in memory. Persistence is what the prototype is _checking_, not something it should lean on. If the question actually involves a database, use a scratch DB or a local file named something like `PROTOTYPE, wipe me`.
4. **Skip the polish.** No tests, no extra abstractions, no error handling beyond what it takes to run. The point is to learn something fast.
5. **Surface the state.** After every action (logic) or tap (UI), show what's live and what just changed, so they can see the choice without flipping back and forth.
6. **Capture it when done.**
   - Put the answer into the real code
   - Keep the prototype on a throwaway branch (not main)
   - Leave a link to that branch on the implementation issue
   - Write down the verdict and the question it settled, in the issue or a commit
   - Main only keeps the decision
