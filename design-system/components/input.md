# Input

## Purpose

Inputs exist to capture user-entered text or numbers in forms. Use an input when the user must type a value (name, email, URL, description, search term, or number). Always pair with a visible label; use placeholder only as a hint, not as a replacement for the label.

**When to use**: Forms (settings, profile, create/edit), search bars, filters, and any single-line or multi-line text entry. For a single choice from a fixed list, prefer a dropdown/select instead.

---

## Structure

- **Label** — Visible text associated with the control via `for`/`id` or by wrapping the control. Required.
- **Control** — `<input>` or `<textarea>` with appropriate type and attributes.
- **Hint** (optional) — Helper text below or near the control; linked via `aria-describedby`.
- **Error message** (optional) — Shown when validation fails; linked via `aria-describedby`; control has `aria-invalid="true"`.

---

## Variants

| Type       | When to use |
|------------|-------------|
| **Text**   | Single-line: name, URL, generic text. `type="text"`. |
| **Email**  | Email address. `type="email"`. |
| **Password** | Secret input; mask characters. `type="password"`; optional show/hide toggle with accessible name. |
| **Number** | Numeric only; optional stepper or min/max. `type="number"`. |
| **Search** | Search field or combobox. `type="search"` or combobox pattern when list is filterable. |
| **Textarea** | Multi-line: bio, description, long content. Optional `rows` and resize behavior. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Default** | Rest border and background from tokens. |
| **Hover**   | Optional subtle border or background change. |
| **Focus**   | Visible focus ring (min 2px) on `:focus-visible`. |
| **Disabled**| Non-editable; reduced opacity; not focusable or editable. |
| **Error**   | Border or outline in error color; `aria-invalid="true"`; error message linked via `aria-describedby`. |
| **Required**| Indicate required (e.g. asterisk or "Required"); `required` and `aria-required="true"`. |

---

## Layout & Spacing

- **Typography**: `typography.fontSize.base`, `typography.fontFamily.sans`.
- **Border**: 1px solid border color; radius (e.g. 0.375rem). Use semantic or primitive tokens; error state uses error/destructive color.
- **Padding**: Vertical and horizontal (e.g. 0.5rem 0.75rem) for comfortable tap/click.
- **Spacing**: Small gap between label and control (e.g. 0.25–0.5rem); medium gap between fields (e.g. 1rem). Reference spacing scale and `design-system/tokens/color-primitives.json` for contrast.

---

## Accessibility

- **Label**: Every input has a visible label programmatically associated (`<label for="id">` or wrapping). Top-aligned label preferred for narrow widths.
- **Describedby**: Link hint and/or error to the control with `aria-describedby` (one id or space-separated ids). When error is shown, include error element id in `aria-describedby` and set `aria-invalid="true"` on the control.
- **Contrast**: Text and border colors meet WCAG 2.2 AA. Error text and border must be clearly visible.
- **Keyboard**: Focusable; full keyboard editing; no trap.

Reference: `.cursor/rules/accessibility.mdc` — global form and validation (e.g. inline validation on blur/submit).

---

## Motion / Behavior

- Optional brief transition (e.g. 150ms) on border/background for focus or error. Respect `prefers-reduced-motion`.

---

## Do's & Don'ts

- **Do** always provide a visible, programmatically associated label.
- **Do** use placeholder as a hint only; never as the only label.
- **Do** show one error message per field and set `aria-invalid` and `aria-describedby` when invalid.
- **Don't** validate on every keystroke for free-text; use blur or submit.
- **Don't** use a div or span to mimic an input; use native `<input>` or `<textarea>`.

---

## Example Usage (AI-Readable)

On a profile form, add an "Email" input: label "Email" above the field, `type="email"`, placeholder "you@example.com", and optional hint "We'll never share your email" linked with `aria-describedby`. When the user leaves the field and the value is invalid, show an error message below the input, set the input's `aria-invalid="true"`, and add the error element's id to `aria-describedby` so screen readers announce the error. Use spacing tokens for the gap between label and input and between this field and the next.
