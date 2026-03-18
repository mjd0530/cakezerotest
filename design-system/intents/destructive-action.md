# Destructive Action

Handle user actions that are irreversible or critical, such as deleting files or accounts. The user must explicitly confirm before the operation runs; cancel restores context with no change.

---

## Meta

```yaml
document_type: intent
intent_id: destructive-action
scope: user_goal
version: 1.0
```

---

## Purpose

The user intends to perform an **irreversible or critical operation** (e.g. delete a file, remove an account, permanently discard data). The system must require explicit confirmation before executing. This intent ensures no accidental confirmation, clear consequences, focus management, and accessible feedback.

**When to use**: Delete/remove actions, account deletion, irreversible resets, or any operation that cannot be undone. Do not use for minor or reversible actions.

---

## Trigger

- User clicks a control labeled as destructive (e.g. "Delete", "Remove", "Delete account").
- System is about to perform an operation that has been flagged as irreversible or critical and requires confirmation.

---

## Expected Outcome

- **User confirms**: The destructive operation runs; modal closes; focus returns to the trigger or next logical element; success or failure feedback is shown accessibly.
- **User cancels**: No action is taken; modal closes; focus returns to the trigger.

---

## Required Components

| Component | Usage |
|-----------|-------|
| Modal (confirmation) | `design-system/components/modal.md` — Confirmation variant: title, body describing consequences, primary destructive + secondary cancel. |
| Buttons | `design-system/components/button.md` — Destructive for primary (e.g. "Delete"); Secondary for cancel (e.g. "Cancel"). |
| Feedback | Success or failure message via `aria-live` region, inline status, or toast so screen readers and sighted users get feedback (see `design-system/policies/accessibility.md` — Agent Actions and Status). |

---

## Interaction Rules

1. **Confirmation modal** — Show a modal with a clear **title** (e.g. "Delete file?") and **body** text explaining consequences (e.g. "This action cannot be undone."). Do not execute the action on the initial trigger click.
2. **Focus trap** — When the modal opens, move focus to the first focusable element inside the dialog (typically Cancel). Tab and Shift+Tab must cycle only inside the modal; focus must not leave until the modal is closed. Per `design-system/policies/accessibility.md` — Modal focus trap.
3. **Primary action** — The destructive button (e.g. "Delete") triggers the operation only when the user activates it. On success: close modal, restore focus to the trigger or next logical element, then show success feedback. On failure: keep modal open or show error in modal/alert; do not close without user acknowledgment.
4. **Secondary action** — Cancel (or Escape) closes the modal without performing the action and restores focus to the element that opened the modal.
5. **Accessibility** — Enforce keyboard operation (Enter/Space on buttons, Escape to close), visible focus ring (min 2px), and screen reader support: dialog announced with `aria-labelledby` / `aria-describedby`; buttons have accessible names; success/failure feedback in a live region or equivalent.

---

## Motion / Behavior Rules

- **Modal**: Animate from center toward user per `design-system/components/modal.md` (e.g. overlay fade-in + panel scale or slight slide, ~200ms ease-out). Respect `prefers-reduced-motion`: skip or significantly shorten animation.
- **Buttons**: Hover and click (active) states animate per `design-system/components/button.md` and `design-system/policies/accessibility.md`; respect `prefers-reduced-motion`.

---

## Do's & Don'ts

| Do | Don't |
|----|--------|
| Highlight the destructive action clearly (e.g. red/destructive button). | Allow accidental confirmation (e.g. single click without modal). |
| Use a confirmation modal with a clear title and body explaining consequences. | Use destructive confirmation modals for minor or reversible actions. |
| Trap focus inside the modal and restore focus on close. | Execute the destructive operation before the user confirms in the modal. |
| Provide success or failure feedback accessibly after the operation. | Rely on motion alone to convey state; support reduced motion. |

---

## Example AI Usage

When a user selects "Delete File", show a destructive-action confirmation modal with title (e.g. "Delete file?") and body (e.g. "This file will be permanently removed."). Use "Delete" as the primary destructive button and "Cancel" as the secondary button. Ensure focus trap inside the modal and proper spacing per modal and button components. On "Delete", perform the operation, close the modal, restore focus to the trigger, and announce success or failure (e.g. "File deleted" or "Could not delete file") in an `aria-live` region or equivalent. On "Cancel" or Escape, close the modal and restore focus to the trigger with no action taken.

---

## Related

- **Confirmation (general)** — `design-system/intents/confirmation.md` (destructive is a variant; this intent specializes for irreversible operations).
- **Accessibility** — `design-system/policies/accessibility.md` — Modal (4.2), Modal Focus Trap (5), Destructive Actions and Confirmation (3.5).
