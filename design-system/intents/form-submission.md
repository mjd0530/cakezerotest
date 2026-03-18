# Intent: Form Submission

## Purpose

The user intends to **submit form data** (save, create, update, or send). The system must validate input, provide clear feedback, and on success either persist data and show confirmation or navigate as appropriate. Failed validation or submission errors must be communicated accessibly.

**When to use**: Settings pages, create/edit flows, search filters, login/signup, and any UI where the user fills fields and triggers a submit action.

---

## Flow

1. **Form** — User interacts with labeled inputs, toggles, selects, etc. (see `components/input.md`, `components/toggle.md`). Group related fields; use fieldsets where appropriate.
2. **Submit trigger** — User activates Submit/Save/Create (primary button). Use `<button type="submit">` when inside a form, or a primary button that triggers submit programmatically.
3. **Validation** — Validate on submit (and optionally on blur). Show inline errors next to fields; ensure errors have accessible associations (`aria-describedby`, `aria-invalid`) and are announced.
4. **Loading** — While submitting, set button to loading state (`aria-busy="true"`, spinner or "Loading" text). Do not disable the whole form unless necessary; avoid trapping the user.
5. **Outcome** — Success: show confirmation (inline message, toast, or redirect). Error: show error message accessibly (e.g. `aria-live="polite"` or inline) and allow retry.

---

## Components to Use

| Element | Reference |
|--------|-----------|
| Buttons | `components/button.md` — Primary for submit; Secondary for Cancel/Reset |
| Inputs, toggles | `components/input.md`, `components/toggle.md` |
| Modals (short forms) | `components/modal.md` — Form variant |
| Layout (settings) | `design-system/pages/settings-page.md` — Sections, alignment, validation |

---

## Accessibility

- **Labels**: Every form control must have an associated visible label or `aria-label`/`aria-labelledby`.
- **Errors**: Associate error text with the control via `aria-describedby`; set `aria-invalid="true"` when the field has an error. Announce errors (e.g. focus first error or use `aria-live` for form-level message).
- **Submit button**: Accessible name (e.g. "Save", "Submit"); when loading, `aria-busy="true"` and include "loading" in name or adjacent status.
- **Success/error feedback**: Use a live region or visible message so screen reader users hear the result; ensure contrast and focus management (e.g. move focus to success message or first error).

---

## Do's & Don'ts

- **Do** use one primary submit button per form or section; pair with Cancel or Reset when appropriate.
- **Do** validate and show errors before or on submit; don't submit invalid data silently.
- **Do** provide clear success and error feedback (visible and announced).
- **Don't** clear the form on validation error without user intent; preserve input when showing errors.
- **Don't** use form submission for destructive actions without a confirmation step (see `intents/destructive-action.md`).

---

## Example Usage (AI-Readable)

- On a settings page, a "Profile" section has fields: Name (text), Email (email). At the bottom, primary button "Save Changes" and secondary "Cancel". On Submit: validate; if invalid, show errors with `aria-invalid` and `aria-describedby`; if valid, set button to loading, submit; on success show "Settings saved" in a live region and optionally keep form editable; on error show "Something went wrong. Please try again." with accessible retry.
- In a modal "Add API key", form with label "Key name" (text) and "Key" (text or paste). Footer: "Cancel" (secondary), "Add key" (primary). On submit: validate; on success close modal and return focus to trigger; on error show message in modal body with `aria-live="polite"`.
