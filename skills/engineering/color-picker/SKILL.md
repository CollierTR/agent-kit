---
name: color-picker
description: >-
  Throwaway floating color picker to choose one live color, then apply it and
  delete the scaffolding. Use when picking, choosing, or trying a color for a
  component, token, CSS variable, brand accent, background, or text — or when
  the user wants a color picker prototype.
---

# Color Picker

A **throwaway floating picker** that answers "which color?" for one live target. Suggest five options, let them pick freely, wait for their hex, then put only that color into the real code and delete the rest.

Sibling of [prototype](../prototype/SKILL.md): same throwaway discipline and floating-button door; this one is only for color.

## Shape

Floating button on the app shell opens a panel tied to the target color or component:

```
┌─────────────────────────┐
│  #AABBCC                │  ← current hex (updates as they pick)
├─────────────────────────┤
│  ■  ■  ■  ■  ■          │  ← five agent-suggested swatches
├─────────────────────────┤
│  [ free color picker ]  │  ← any color
└─────────────────────────┘
```

- **Very top**: live hex of the current selection (readable, copyable).
- **Under that**: five swatches you chose for this context. Tap one → it becomes current and paints the live target.
- **Bottom**: a free color picker (`<input type="color">` or equivalent). Any pick updates hex + live target.
- **Floating button**: labeled as a color-picker prototype; hide it while the panel is open.
- Selection lives in memory and drives the real target immediately (token, style, prop — whatever is under decision).

Name files/routes so they are obviously throwaway. Follow the project's routing and shell; don't invent a new top-level structure.

## Process

### 1. Identify the target

- Name the exact color under decision: CSS variable, Tailwind class, theme token, inline style, or component prop.
- Default the picker to the **current production value** so they have a baseline.
- One picker ↔ one target. If several colors are in play, run separate pickers (or ask which one first).

### 2. Suggest five

- Pick five options from context: existing palette, brand, contrast against neighbours, light/dark surfaces, and one deliberate stretch.
- Short labels optional; the swatch and hex matter more.
- Put the five in the panel; don't make them hunt through a long list.

### 3. Build the door and the panel

- Floating button on the app shell → opens the panel.
- Wire every swatch and the free picker to the in-memory current color; the live target reads that value.
- Mark throwaway files with a top-of-file comment.
- Skip tests, extra abstractions, and production error handling.

### 4. Hand over and wait

- Tell them where the button is and that the hex at the top is the answer to send back.
- **Stop and wait** for the user to provide the color (paste the hex, or name a swatch). Do not clean up, commit, or "pick for them" while waiting.

### 5. Apply and clean up

- Set the real target to the chosen color only.
- Delete the floating button, panel, temporary wiring, and throwaway files — working tree matches pre-picker code aside from the color change.
- Main keeps the decision, not the picker.

## Anti-patterns

- **Picker without a live target.** The point is seeing the color on the real chrome, not a swatch in isolation.
- **Cleaning up before they answer.** Wait for the hex (or explicit "keep #…").
- **Leaving scaffolding.** After apply, no prototype button, route, or temp state remains.
- **Five options that ignore context.** Derive suggestions from the surrounding UI and palette, not a generic rainbow.
