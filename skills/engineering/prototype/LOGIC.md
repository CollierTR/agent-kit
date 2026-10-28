# Logic Prototype

A single HTML file (a **shareable demo**) that lets anyone drive a state model by clicking buttons. Use this when the question is about **business logic, state transitions, or data shape**: the kind of thing that looks fine on paper and only feels wrong once you push it through real cases.

Because it's one file with nothing to install, you can hand it to a non-developer (a designer, a PM, a domain expert) and let them feel the model themselves. So it speaks their language, not the code's.

## When this is the right shape

- "I'm not sure if this state machine handles the edge case where X then Y."
- "Does this data model actually let me represent the case where..."
- "I want to feel out what the API should look like before writing it."
- Anything where someone wants to **press buttons and watch state change**.

If the question is "what should this look like," this is the wrong branch. Use [UI.md](UI.md).

## Process

### 1. State the question

- Before writing code, write down which state model you're prototyping and what question it should answer
- One paragraph at the top of the demo, in a visible intro — not just a comment

A logic prototype that answers the wrong question is a waste, so make the question checkable later, whether they're watching now or coming back to it AFK.

### 2. Isolate the logic in a portable module

- Put the actual logic (the bit answering the question) in a single `<script>` block, as a small pure module you could lift into the real codebase later
- The page around it is throwaway; this module isn't

The right shape depends on the question:

- **A pure reducer**: `(state, action) => state`. Good when actions are discrete events and state is a single value.
- **A state machine**: explicit states and transitions. Good when "which actions are even legal right now" is part of the question.
- **A small set of pure functions** over a plain data type. Good when there's no implicit current state, just transformations.
- **A class or module with a clear method surface** when the logic genuinely owns ongoing internal state.

- Pick the shape that fits the question, not whichever is easiest to wire to a page
- Keep it pure: no DOM, no `document`, no button handlers reaching inside it
- The page calls into it; nothing flows the other way

That's what makes the prototype useful later: once the question's answered, the reducer / machine / functions drop into the real module on their own.

### 3. Build the shareable HTML file

- One file, plain HTML/CSS/JS: no framework, no bundler, no server
- Everything inline so it opens with a double-click and survives being emailed around
- Write it for a non-developer
- Every label is in **domain language**, not code: buttons and state read like the business, not the reducer
- Explain in plain words what's happening

Lay it out top to bottom:

1. **Title and one-line explanation** of what this demo lets you explore (the question from step 1).
2. **Current state**: the full relevant state, shown as a readable panel (labelled fields, not a raw JSON dump), re-rendered after every click so the change is visible. Where it helps a non-developer follow, call out what just changed.
3. **Free-play buttons**: one button per action, always available, so anyone can poke at the model in any order. Each click dispatches its action and re-renders the state.
4. **Guided walkthroughs**: a set of **scenarios**, one per tab.
   - Short plain-language description (the situation it sets up and what to watch for)
   - Ordered **buttons to press** underneath
   - Each step is a real button: clicking it performs that action and moves to the next step
   - Starting a walkthrough resets to a known initial state so the scenario runs the same way every time

- Pick scenarios that show the awkward cases: the happy path, a tricky edge case, an attempt at something that should be illegal
- Keep it nice but quiet: clean typography, generous spacing, one accent colour
- No animations, no gimmicks. Nothing that competes with the state and the buttons.

### 4. Hand it over

- Send them the file, or open it for them
- If they want new actions or a new scenario, add them

They'll click through the walkthroughs and free-play whenever they get to it. The interesting moments are "wait, that shouldn't be possible" or "huh, I assumed X would be different" — those are bugs in the _idea_, which is the whole point. Prototypes evolve.

### 5. Capture the answer and the prototype

- Capture the answer, then the prototype, the way the [SKILL](SKILL.md) describes
- Lift the reducer / machine / functions into the real module
- Keep the HTML file on the throwaway branch so you can still open it later

## Anti-patterns

- **Tests.** Skip them. A prototype that needs tests isn't a prototype.
- **The real database.** Keep state in memory unless the question is about persistence.
- **Extra generality.** Skip "what if we wanted to support X later." The prototype answers one question.
- **Logic mixed into the page.** If the module touches the DOM, `document`, or button handlers, you can't lift it. Keep the page as a thin shell over a pure module.
- **A framework, bundler, or server.** One file they double-click. A React app or a dev server isn't shareable.
- **Shipping the HTML into production.** The page is for clicking through by hand. The logic module behind it is the bit worth keeping.
