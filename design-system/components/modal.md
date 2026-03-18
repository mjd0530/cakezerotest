# Modal (Dialog)

## Purpose

Display content that requires user attention without leaving the current page. Modals focus the user on a single task or decision—confirming a destructive action, completing a short form, or acknowledging a critical message. Use a modal when the user must respond before continuing; avoid putting primary settings or long flows inside modals.

**When to use**: Confirmations (delete, overwrite), short add/edit forms, critical alerts, or optional wizards. Prefer inline content or a dedicated route for complex, multi-step flows.

---

## Structure

- **Overlay** — Semi-transparent backdrop covering the page; optional click-to-close.
- **Panel** — Container with surface background, border radius, and shadow.
- **Header** — Title (required) and optional close icon. Use typography token for h2 (e.g. `typography.headlineS` or `typography.headlineM`). Give the title a stable `id` for `aria-labelledby`. Close icon: icon-only button with `aria-label="Close"` (Material icon `close`).
- **Body** — Main content; optional short description or custom content. Use an `id` for `aria-describedby` when present.
- **Footer** — Action buttons aligned right (e.g. Confirm + Cancel, or Save + Cancel). Use spacing tokens for gap between buttons.

---

## Variants


| Variant           | When to use                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Confirmation**  | Destructive or irreversible action only. Title, short description, Confirm + Cancel. No extra form fields.                            |
| **Informational** | Critical alerts, notices, or acknowledgments. Title, body text, and a single primary action (e.g. OK, Dismiss) or optional secondary. |
| **Form**          | Short add/edit flows, multi-step wizards. Title, body with form fields, primary + secondary actions (e.g. Save + Cancel).             |


---

## Interaction States


| State     | Behavior                                                                                                                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Open**  | Focus moves to first focusable element inside the dialog (or panel if none). Focus trap active.                                                                                                               |
| **Focus** | Tab and Shift+Tab cycle only through focusable elements inside the modal; focus does not leave the dialog.                                                                                                    |
| **Close** | Escape closes the modal and returns focus to the element that opened it. Optional: click outside (overlay) to close when enabled. Cancel/Confirm or close icon also closes and restores focus to the trigger. |


---

## Layout & Spacing

- **Overlay**: Semi-transparent background (e.g. `primitive.common.black` at ~50% opacity or rgba equivalent).
- **Panel**: Surface color from tokens; border radius (e.g. 0.5rem); elevation shadow. Max width as needed (e.g. 480px for confirmation, wider for forms).
- **Header**: Typography token for h2 (e.g. `typography.headlineS` or `typography.headlineM`). Use spacing tokens for padding and margin (e.g. 1.5rem).
- **Body**: `typography.base` or equivalent for body text. Consistent padding via spacing tokens; gap between header, body, and footer.
- **Footer**: Buttons aligned right; use spacing tokens for padding and gap between buttons (e.g. `sm`, `md`).
- Reference spacing scale in tokens (e.g. `xs`, `sm`, `md`, `lg`) for all padding and margins.

---

## Accessibility

- **Roles**: `role="dialog"` (or native `<dialog>` where supported), `aria-modal="true"`.
- **Labels**: `aria-labelledby` points to the title element id; `aria-describedby` points to description/body id when present.
- **Focus trap**: When open, Tab/Shift+Tab cycle only inside the dialog; focus must not move to page content until the modal is closed.
- **Escape**: Closes the modal and returns focus to the trigger element.
- **Restore focus**: On close (Escape, overlay click, Cancel, Confirm, or close icon), return focus to the element that opened the modal.
- **Descriptive header**: Provide a visible, concise title that describes the purpose of the dialog.

Reference: `.cursor/rules/accessibility.mdc` — Modal (Section 4.2), Modal Focus Trap (Section 5).

---

## Motion / Behavior

- **Enter**: Animate from center toward user using ease-out over 200ms (e.g. overlay fade-in + panel scale or slight slide). Respect `prefers-reduced-motion`: skip or significantly shorten animation.
- **Exit**: Matching exit transition; after transition, remove from DOM and restore focus.

---

## Do's & Don'ts

- **Do** use for critical actions (confirmations, short forms, critical alerts).
- **Do** provide a visible title and, for confirmation modals, a short description.
- **Do** trap focus inside the dialog and restore focus to the trigger on close.
- **Do** close on Escape and provide a Cancel or close control (and optional close icon in header).
- **Don't** overuse for minor alerts; prefer inline messages or toasts when appropriate.
- **Don't** allow Tab to move focus to the page behind the modal.
- **Don't** close without restoring focus to the trigger.
- **Don't** use modals for long forms or primary app settings; use a full page or drawer when appropriate.

---

## Example Usage (AI-Readable)

- **Confirmation**: "Show a confirmation modal with header 'Delete File', body 'Are you sure?', primary button 'Delete', secondary button 'Cancel'."
- **Detailed**: When the user clicks "Delete device" in a table row, open a confirmation modal. Set the modal title to "Delete device?" and the description to "This action cannot be undone." Provide two buttons: "Cancel" (secondary, closes modal and returns focus to the Delete button) and "Delete" (destructive, performs delete then closes modal and returns focus). When the modal opens, move focus to the first focusable element (e.g. Cancel). Trap focus so Tab cycles only between Cancel and Delete. When the user presses Escape or clicks Cancel, close the modal and return focus to the "Delete device" button in the table row.

