# Intent: Outcome feedback (confirmation)

Confirm to the user that an important **non-destructive** action succeeded or failed. Use for form submissions and significant system changes where the user needs clear success or error feedback.

---

## Meta

```yaml
document_type: intent
intent_id: outcome-feedback
scope: user_goal
version: 1.0
```

---

## Purpose

Confirm a user's intent for important but **non-destructive** actions by showing that the action succeeded or failed. Use when the user has already performed an action (e.g. submitted a form, saved settings, triggered a significant change) and the system must communicate the result clearly.

**When to use**: After form submit, after saving or updating data, after performing a significant system change. Do not overuse for trivial actions (e.g. every tiny toggle); reserve for actions that warrant explicit feedback.

---

## Trigger

- Submitting a form (save, add, update).
- Performing a significant system change (e.g. apply settings, sync, publish).

---

## Expected Outcome

- **Success**: Display a success message (toast or modal with a single acknowledgment action, e.g. "OK").
- **Failure**: Display an error message (toast or modal with clear error text and optional retry/cancel).

---

## Required Components

| Component | Usage |
|-----------|-------|
| Modal | `components/modal.md` — Informational variant for success/error with title, body, and single primary button (e.g. OK, Dismiss). Use when the outcome is critical or the user must acknowledge. |
| Toast | For non-blocking success/error feedback when the user can continue without dismissing. Slide in/out from bottom; see Motion below. |
| Buttons | `components/button.md` — Primary for "OK" / "Dismiss"; optional secondary for "Retry" on error. Follow design system rules. |
| Alerts / status | Inline or in modal body for error text; use `aria-live="polite"` for dynamic success/error so screen readers announce the result. |

---

## Interaction Rules

1. **Show feedback immediately** after the user action (e.g. right after submit).
2. **Success**: Show a clear success message (e.g. "Profile updated successfully") and a single acknowledgment button (e.g. "OK"). On OK, close modal or dismiss toast and restore focus if a modal was used.
3. **Failure**: Show a clear error message (e.g. "Could not save. Please try again.") and optional "Retry" or "Cancel". Buttons and messaging must follow design system rules; ensure accessible names and keyboard operability.
4. **Accessibility**: Per `design-system/policies/accessibility.md` — success/error must be announced (e.g. `aria-live="polite"`, `role="status"`). Modal focus trap and focus restore on close when using a modal.

---

## Motion / Behavior Rules

- **Toasts**: Slide in/out from bottom. Respect `prefers-reduced-motion`: reduce or remove slide animation when user prefers reduced motion (per accessibility policy).
- **Modals**: Animate per modal component and motion policy (e.g. overlay fade, panel enter/exit). Respect `prefers-reduced-motion` for any entrance/exit animation.

---

## Do's & Don'ts

| Do | Don't |
|----|--------|
| Confirm intent without causing friction; use toasts when a modal is unnecessary. | Overuse for trivial actions (e.g. every small toggle). |
| Provide clear success or error messaging and one primary action (OK / Dismiss). | Use multiple competing live regions; prefer one clear status announcement. |
| Use modal for critical or must-acknowledge outcomes; use toast for lightweight feedback. | Steal focus during streaming or typing; only move focus when opening a modal. |

---

## Example AI Usage

- After the user submits the profile form, show a confirmation intent modal saying "Profile updated successfully" with a single "OK" button. On OK, close the modal and return focus to the trigger or main content.
- After save fails, show an error modal with title "Could not save" and body "Your changes could not be saved. Please check your connection and try again." Buttons: "Retry" (primary), "Cancel" (secondary). Announce the error in a live region when the modal opens.
- For a non-critical success (e.g. "Settings synced"), use a toast that slides in from the bottom with the message "Settings synced" and auto-dismiss or a dismiss control; do not use a modal.
