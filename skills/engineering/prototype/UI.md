# UI Prototype

Build a **comparison page** — every variant on screen at once, labeled, tappable. A floating button on the app shell opens it from anywhere. Back takes them to the last page so they can see the live variant on the real chrome. Then throw the rest away.

If the question is about logic or state rather than how something looks, this is the wrong branch. Use [LOGIC.md](LOGIC.md).

## When this is the right shape

- "What should this control / header / page look like?"
- "I want to see a few options before committing."
- "Try a different icon / selection mark / layout."
- Any time the user would otherwise pick between mockups in their head.

## Shape

- A **throwaway route** next to the real page, named so it's obviously a prototype (`mode-icon-prototype`, `/retail/mode-selection-prototype`)
- Follow the project's routing. Don't invent a new top-level structure.
- A **floating button** on the app shell (root layout / navigator) so any production page can open the selector
- Label the button as a prototype. Hide it on the selector itself.
- A **back** control on the selector that returns to the page they opened it from, using whatever back navigation the app already has (pop the stack, `history.back()`, etc.)
- If you're deciding something that lives in the chrome (header actions, the actual dropdown), put that real chrome on the selector too. The page body is the samples.

```
App shell (every production page)
---------------------------------
[ real page content ]     [ prototype ]
                                 ↑ floating button

Selector
--------
[ ← back ]  Title + one-line question

Real header (updates when they tap)
----------------------------------
[ labeled sample A ]
[ labeled sample B ]  ← "— using this"
[ labeled sample C ]
```

- Each sample looks like the real chrome you're deciding on (a fake header row, a fake menu) — not a swatch or an icon in a list
- Short name and one line of why it exists
- **Tap a sample (or its title) to make it live** on the real chrome — on the selector and on every page that uses that control
- Mark the live one with "— using this" (or an accent border)
- If rows inside a sample should drive real state (e.g. change the selected mode), let them
- Selection lives in memory and is read by the production control
- Stack the samples so the comparison is visible; tapping one makes it live

## Process

### 1. State the question and pick N

- Write the question in one line at the top of the prototype file:

  > "Compare mode-dropdown header icons. Tap a sample to use it in the real header."

- Start with about 3–7 options. Add more when they ask.
- Chrome questions (icon, check mark, frame) can share structure and differ only in treatment.
- Page-layout questions should still disagree about hierarchy, not just colour.

### 2. Build the samples

- Use the project's components and tokens.
- Keep samples faithful to the real density and neighbours (barcode next to the mode icon, three mode rows in the menu).
- Skip tests, extra abstractions, and production error handling.
- Mark the files as throwaway in a top-of-file comment.

### 3. Wire the live variant and the door

- Point the production control at the in-memory selection so tapping a sample updates the real chrome right away — on the selector and on every page that uses that control
- Default to the current production look so they have a baseline
- Put the floating button on the app shell so any production page can open the selector
- Put a back control on the selector that returns to the page they came from

### 4. Hand it over

- Tell them where it is
- How to pick: tap the floating button, tap a sample, then back to the last page to see it live
- Say which one to keep

### 5. Capture the answer and clean up

- Copy the winner into the real control
- Delete the prototype route, floating button, and temporary wiring from the working branch
- Main only keeps the decision
- If you want to keep the full set of variants, commit them on a throwaway branch and leave a link on the issue — not sitting in main

## Anti-patterns

- **One-at-a-time flipper.** A `?variant=` bar or floating pager of variants hides the comparison. Stack the samples. The floating button opens the selector; it isn't a variant switcher.
- **Swatches without chrome.** An icon grid is not a header. Rebuild the real neighbours.
- **Samples on the production page.** The selector holds the comparison; the floating button is only how you get there. Don't park six fake headers on the live retail form.
- **Promoting the prototype as-is.** Rewrite the winner into the production control. The sample code was throwaway.
