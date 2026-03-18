# Intent: Confirmation

## Purpose

The user must **explicitly confirm or cancel** a decision before the system proceeds. Confirmation is used for destructive actions (see `intents/destructive-action.md`) and for other high-stakes or reversible-but-significant choices where a single click would be risky (e.g. overwrite, publish, submit payment).

**When to use**: Before destructive actions (required), before overwriting data, before publishing or going live, or when the user needs a clear "Are you sure?" step. Do not overuse for low-risk actions.

---

## Flow

1. **Trigger** — User invokes an action that requires confirmation (e.g. "Delete", "Overwrite", "Publish").
2. **Modal** — Open a confirmation modal with: a **title** that states the decision (e.g. "Delete device?"), an optional **description** that explains consequence or irreversibility, and **two actions** — one to confirm (primary or destructive as appropriate) and one to cancel (secondary).
3. **Focus** — Move focus into the modal (first focusable element, often Cancel). Trap focus; Tab cycles only inside the dialog.
4. **Outcome** — On Confirm: perform the action, close modal, restore focus to trigger or next logical element. On Cancel or Escape: close modal, no action, restore focus to trigger.

---

## Components to Use

| Element | Reference |
|--------|-----------|
| Confirmation modal | `components/modal.md` — Confirmation variant |
| Buttons | `components/button.md` — Primary or Destructive for confirm; Secondary for cancel |
| Focus trap & restore | `design-system/policies/accessibility.md` — Modal focus trap |

---

## Variants

| Scenario | Title style | Confirm button | Cancel |
|----------|-------------|----------------|--------|
| Destructive (delete, remove) | "Delete X?" | Destructive ("Delete") | Secondary ("Cancel") |
| Overwrite / replace | "Overwrite file?" | Primary or Destructive | Secondary ("Cancel") |
| Publish / go live | "Publish changes?" | Primary ("Publish") | Secondary ("Cancel") |
| Reversible but significant | "Discard unsaved changes?" | Primary or Destructive | Secondary ("Keep editing") |

---

## Accessibility

- **Dialog**: `role="dialog"`, `aria-modal="true"`, `aria-labelledby` (title id), `aria-describedby` (description id when present).
- **Focus**: Focus moves to first focusable element when modal opens; focus trap; on close, focus returns to the element that opened the modal.
- **Escape**: Closes the modal and cancels (no action); focus restored to trigger.
- **Labels**: Confirm and Cancel buttons must have clear, accessible names.

---

## Do's & Don'ts

- **Do** provide a visible title and, when the action is destructive or irreversible, a short description.
- **Do** use one primary/destructive confirm and one secondary cancel; avoid more than two actions unless the flow explicitly requires it.
- **Do** trap focus and restore focus on close.
- **Don't** use confirmation modals for trivial actions (e.g. "Close without saving?" when no changes exist).
- **Don't** allow the underlying action to run without the user explicitly choosing Confirm.

---

## Example Usage (AI-Readable)

- Confirmation for delete: Modal title "Delete device?", description "This action cannot be undone." Buttons: "Cancel" (secondary), "Delete" (destructive). Focus trap; Escape or Cancel closes and restores focus to Delete button; Confirm deletes and closes, then restores focus.
- Confirmation for discard: Modal title "Discard unsaved changes?", description "Your changes will be lost." Buttons: "Keep editing" (secondary), "Discard" (primary or destructive). On Discard: navigate away or clear form; on Keep editing or Escape: close modal and return focus to trigger.
