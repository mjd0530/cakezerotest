# Accessibility Policy — Agentic Design System

<!-- Design system policy: accessibility. Config and full rules. The agent uses .cursor/rules/accessibility.mdc when generating UI; this doc is the human-facing reference. -->

---

## Policy config

Use these values when implementing components:

| Key | Value | Meaning |
|-----|--------|--------|
| `reducedMotion` | `noScale` | When `prefers-reduced-motion: reduce`, disable or reduce non-essential motion; do not scale motion in a way that disorients. |
| `focusTrapModals` | `true` | Modals must trap focus (Tab/Shift+Tab stay inside); restore focus to trigger on close. |
| `focusRingMinWidth` | `2` | Visible focus ring minimum width 2px (e.g. via `:focus-visible`). |

---

## 1. Why Agentic Accessibility Is Different

Standard WCAG and ARIA guidance assume **user-triggered**, **deterministic** interactions. In agentic interfaces, content and state often change without a direct user action: AI generates or streams text, background tasks run, and the agent performs actions on the user's behalf. This section reframes accessibility for that reality.

### 1.1 POUR Principles Reframed for AI Interfaces

| Principle | Traditional | Agentic reframe |
|-----------|-------------|------------------|
| **Perceivable** | Information and UI are presentable to users in ways they can perceive. | **AI-generated and streamed content** must be exposed to assistive tech as it appears. Streaming text, status updates, and non-deterministic output need live regions, incremental announcements, or structured updates so users perceive what the system is doing. |
| **Operable** | UI components and navigation are operable. | **Long-running and background tasks** must not block or trap the user. Provide **interrupt** and **cancel** where appropriate. Focus and tab order must remain predictable even when content updates. Avoid keyboard traps except in controlled contexts (e.g. modals). |
| **Understandable** | Information and operation of the UI are understandable. | **Agent actions** must be clearly described: what the agent did, is doing, or will do. Use plain language and consistent patterns. **Non-deterministic outcomes** should be explained (e.g. "Searching…", "Found 3 results", "No matches"). |
| **Robust** | Content is robust enough for assistive technologies. | **Dynamic and streamed content** must use correct semantics and ARIA so screen readers and other AT can parse and announce it. Mark up structure (headings, lists, regions) even when content is generated or updated asynchronously. |

### 1.2 Five Agentic-Specific Principles

1. **Transparency** — The user must always know what the system is doing. Announce agent activity (e.g. "Agent is searching"), progress (e.g. "Step 2 of 4"), and results. Use `aria-live` and clear labels; avoid silent background changes that only sighted users would notice.

2. **Interruptibility** — Long-running or multi-step agent tasks should be interruptible where safe. Provide "Cancel" or "Stop" that is keyboard accessible and clearly labeled. Do not force users to wait without feedback or control.

3. **Predictability** — UI changes triggered by the agent (new content, modals, focus shifts) must be predictable. Avoid surprising focus moves or unexpected dialogs. When focus must move (e.g. into a new modal), announce the context change.

4. **Recoverability** — Destructive or irreversible agent actions require explicit user confirmation (see destructive actions below). Errors and failures must be communicated accessibly and offer a way to retry or recover.

5. **Cognitive load** — Reduce ambiguity. Use consistent patterns for "agent is working," "agent finished," and "agent needs input." Avoid overwhelming users with too many live announcements; prioritize the most important status (e.g. one primary `aria-live` region for current operation).

---

## 2. Standards Reference

- **WCAG 2.2 Level AA** — Required. All content and UI must meet Level AA success criteria.
- **ARIA Authoring Practices Guide (APG)** — Required. Use APG patterns for custom components (dialogs, buttons, feeds, etc.).
- **Section 508** — Where applicable (federal or contractual), align with Revised Section 508 (WCAG 2.0 Level AA as the baseline; WCAG 2.2 AA satisfies and extends this).
- **WCAG 2.2 Level AAA** — Note criteria worth pursuing where feasible (e.g. 2.4.8 Location, 2.4.9 Link Purpose, 3.2.5 Change on Request). Not required for baseline compliance.

---

## 3. Global Rules

### 3.1 Color and Contrast (WCAG 2.2)

- Text on background: **≥ 4.5:1** for normal text, **≥ 3:1** for large text (18pt+ or 14pt+ bold).
- Use design token colors that are verified for contrast (e.g. `color.text` on `color.surface`, `color.primary` on white for buttons).
- Destructive actions must use a color that meets contrast on the background (e.g. `color.destructive`).
- **AAA note:** Aim for **≥ 7:1** for normal text where design allows.

### 3.2 Screen Reader Compatibility

- Every interactive element must have an **accessible name**: visible label or `aria-label` (or `aria-labelledby` where appropriate).
- Icon-only buttons must have `aria-label` describing the action.
- Modals must be announced when opened: use `role="dialog"`, `aria-modal="true"`, and `aria-labelledby` / `aria-describedby` so the title and description are read.
- For **dynamic/streaming content**, use `aria-live` regions with an appropriate politeness (`polite` default, `assertive` only for urgent interruptions). Ensure live regions have a consistent, predictable structure.

### 3.3 Keyboard Navigation

- All interactive elements must be **focusable** (in tab order) and **activatable** with Enter and Space where appropriate.
- Focus order must follow a logical sequence (e.g. top to bottom, left to right).
- **No keyboard traps** except inside modals; see Modal focus trap below.
- **Escape** should dismiss overlays, cancel in-progress flows, or close modals and return focus.
- Visible focus ring: minimum 2px (per `focusRingMinWidth`), using `:focus-visible`.

### 3.4 Motion and Animation

- Respect **prefers-reduced-motion**. When `prefers-reduced-motion: reduce` is set, apply `reducedMotion: noScale`: disable or significantly reduce non-essential motion (e.g. loading spinners can remain; avoid decorative animations that could cause discomfort).
- Do not rely on motion alone to convey meaning; provide text or icons as well.

### 3.5 Destructive Actions and Confirmation

- **Destructive actions** (e.g. Delete, Remove, irreversible reset) must **not** execute immediately from a single button click.
- The agent must generate a **confirmation modal** when the intent is destructive: the user must explicitly confirm (e.g. "Delete" / "Cancel") before the action runs.
- The modal must include a clear **title** and **description** (e.g. "Delete device?" and "This action cannot be undone.").

---

## 4. Component Guidelines

For each component: semantic HTML/ARIA, keyboard behavior, screen reader expectations, motion notes, and ✅ Do / ❌ Don't examples.

### 4.1 Button

- Use `<button type="button">` (or `type="submit"` in forms). Provide an accessible name (visible text or `aria-label` for icon-only). Loading: `aria-busy="true"` and optionally label with "loading". Focus ring min 2px; activatable with Enter/Space.
- ✅ `<button type="button" aria-label="Save">Save</button>` ✅ Icon-only: `aria-label="Delete device"` then confirmation modal. ❌ `<div role="button">` or icon-only without `aria-label`.

### 4.2 Modal (Dialog)

- `role="dialog"`, `aria-modal="true"`, `aria-labelledby` / `aria-describedby`. **Focus trap:** Tab/Shift+Tab cycle only inside modal; on close, return focus to trigger. Escape closes and restores focus.
- ✅ Focus trap; visible title and description. ❌ Tab leaving dialog; closing without restoring focus.

### 4.3 Table

- Semantic `<table>`, `<th scope="col">` (or `scope="row"`), optional `<caption>` or `aria-label`. All row actions focusable with clear `aria-label`. ❌ `<div>` grid for tabular data; missing scope/headers.

### 4.4 Streaming / Progressively Revealed Content

- `aria-live="polite"` container; `aria-busy="true"` while streaming, `false` when done. Don't steal focus as content streams. Batch announcements to reduce cognitive load. ✅ `aria-atomic="false"` for long streaming. ❌ `assertive` for normal streaming; auto focus on each token.

### 4.5 Long-Running Tasks and Progress

- Determinate: `role="progressbar"` with `aria-valuenow` / `aria-valuemin` / `aria-valuemax`. Indeterminate: `role="status"` and `aria-live="polite"` with label ("Loading…"). Provide Cancel/Stop button, focusable. ✅ Announce progress and result. ❌ Long task with no progress, cancel, or status.

### 4.6 Agent Actions and Status

- Dedicated status region with `aria-live="polite"` or `role="status"`. Clear language ("Agent is searching", "Found 3 results"). After agent action, announce what was done; offer "Undo"/"Retry" when relevant. One primary live region. ❌ Silent updates; multiple assertive regions.

---

## 5. Modal Focus Trap (Summary)

- On open: focus to first focusable element in dialog (or dialog container).
- Tab and Shift+Tab cycle only inside the modal.
- On close (Escape, Cancel, or Confirm): return focus to the trigger element.

---

## 6. Quick Reference for the Agent

When generating UI:

- Use semantic HTML and ARIA per component guidelines (button, modal, table, streaming, progress, status).
- Every interactive element: accessible name and keyboard operable.
- Modal focus trap and restore focus on close (`focusTrapModals: true`).
- Focus ring min width 2px (`focusRingMinWidth: 2`).
- Destructive intent: confirmation modal with title, description, Confirm/Cancel.
- Streaming/long-running: `aria-live` / `role="status"` and cancel where appropriate.
- Respect `prefers-reduced-motion` (`reducedMotion: noScale`).
- Design token colors meeting WCAG 2.2 AA contrast.

Refer to component docs (`components/button.md`, `components/confirmation-modal.md`, `components/basic-modal.md`, `components/table.md`) for layout and tokens; this policy defines the accessibility requirements those components must satisfy.
