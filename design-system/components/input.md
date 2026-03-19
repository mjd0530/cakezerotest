# Input — Complete Component Spec

<!-- Design system component: text fields and textarea. AI-readable. Use design-system tokens only; map each spec property to the correct CSS property (see policies/tokens-and-css.md). Reference: accessibility.md, iconography.md, policies/typography.md, intents/form-submission.md. -->

---

## Meta

```yaml
document_type: component_spec
component_id: input
scope: forms | filters | search | settings
audience: ai_consumers | developers | designers
version: 2.1
figma_source: MCP get_design_context on selected Input / Text field component (variable defs aligned to semantic tokens)
token_sources:
  - design-system/tokens/color-semantic.json
  - design-system/tokens/typography.json
  - design-system/tokens/spacing.json
  - design-system/tokens/color-primitives.json
spacing_scale: space.xs | space.sm | space.md | space.lg | space.xl | space.2xl | space.3xl (see design-system/tokens/spacing.md)
radius: radius.sm | radius.md (use var(--radius-sm) for control corners per Figma 4px default)
```

---

## 1. Purpose

Inputs capture user-entered text or numbers in forms. Use an `<input>` or `<textarea>` when the user must type a value (name, email, URL, description, search term, or number). Always pair with a **visible label**; use placeholder only as a hint, not as the label.

**When to use**: Forms (settings, profile, create/edit), search bars, filters, and single-line or multi-line text entry. For a single choice from a fixed list, prefer `components/dropdown.md`.

**When not to use**: Do not use `<div contenteditable>` or styled `<span>` as a substitute for native inputs. Do not rely on placeholder alone for the accessible name.

---

## 2. Structure

| Part | Required | Description |
|------|----------|-------------|
| **Label** | Yes (unless `aria-label` on control with design approval) | Visible text; `for`/`id` association or wrapping. |
| **Control** | Yes | Native `<input>` or `<textarea>` with correct `type` and attributes. |
| **Hint** | Optional | Helper below or near control; `id` linked via `aria-describedby`. |
| **Error message** | When invalid | Below control; `aria-describedby` + `aria-invalid="true"` on control. |
| **Success message** | Optional | Post-validation positive feedback; link with `aria-describedby` when it describes the field state. |
| **Trailing affordance** | Optional | Icon button (e.g. date picker trigger, password visibility). Must have accessible name; **Material icons only** per `policies/iconography.md`. |

---

## 3. Variants (input types)

| Type | When to use | Notes |
|------|-------------|--------|
| **Text** | Generic single-line. | `type="text"`. |
| **Email** | Email address. | `type="email"`. |
| **Password** | Secret input. | `type="password"`; optional show/hide **toggle as `<button type="button">`** with `aria-label` (e.g. "Show password"). |
| **Number** | Numeric entry. | `type="number"`; optional `min` / `max` / `step`. |
| **Search** | Search field. | `type="search"`; clear behavior optional. |
| **Textarea** | Multi-line. | `textarea`; `rows` and resize policy per product. |

Visual styling (border, states, sizes) is **shared** across types unless a type-specific note is listed below.

---

## 4. Sizes (unified)

Default dimensions match Figma **Input / Textarea** (height 40px). Use token-backed `min-height`, `padding`, and typography — not Tailwind `text-*` for font-size (those set **color**; see `policies/tokens-and-css.md`).

| Size | Control min-height | Padding (top / right / bottom / left) | Value typography | Line-height (value) |
|------|--------------------|----------------------------------------|------------------|---------------------|
| **Medium** (default) | `2.5rem` (40px) | `var(--space-sm)` `var(--space-sm)` `var(--space-sm)` `var(--space-md)` | `font-size: var(--text-sm); font-weight: 400` | `line-height: var(--line-height-body-m)` (from `typography.json` → `typography.lineHeight.bodyM`) |
| **Small** | `2rem` (32px) | Tighter horizontal/vertical using same scale (`var(--space-sm)` / `var(--space-xs)`) | `font-size: var(--text-xs); font-weight: 400` | `line-height: var(--line-height-body-s)` or `bodyXs` per `typography.json` |

- **Label typography** (all sizes): `font-size: var(--text-sm); font-weight: 600; line-height: var(--line-height-body-m); font-family: var(--font-sans)`.
- **Required asterisk** (when `required`): `color: var(--color-danger)` (semantic `danger.default`).
- **Border radius**: `border-radius: var(--radius-sm)` (Figma 4px control corners).

---

## 5. Layout and spacing

| Relationship | Token / rule |
|--------------|----------------|
| Label stack gap (label → control) | `gap: var(--space-xs)` (Figma 4px). |
| Control internal gap (value ↔ trailing icon) | `gap: var(--space-sm)` (Figma 8px). |
| Hint or validation row gap (icon ↔ text) | `gap: var(--space-sm)`. |
| Field spacing (between inputs in a form) | `margin-bottom` or stack `gap: var(--space-md)` or `var(--space-lg)` per `forms-page.md` / layout context. |

---

## 6. Interaction states (control surface)

Apply to the input container (wrapper that draws border/background) and native control text/placeholder colors. Do not use `:focus` alone for focus ring; use `:focus-visible` per accessibility policy.

### 6.1 Layout stability (required)

Controls **must not** change outer size when the user moves focus into the field.

- Use **`border-width: 1px` for every state** (default, hover, focus, disabled, error, success). **Never** increase border width on `:focus-visible` (e.g. do not switch from `1px` to `2px` or `3px`) — that shrinks/grows the content box and causes layout shift.
- Communicate focus with **`border-color`** change (e.g. to `--color-primary`) **and** a **focus ring** via `outline` (preferred, with `outline-offset`) or an outer **`box-shadow`** ring that does not affect layout. The ring must meet minimum **2px** visible indicator per accessibility policy; that thickness applies to **outline/ring**, not to swapping a thicker border.
- Use **`box-sizing: border-box`** on the control so padding + border are included in `min-height` / width.

### 6.2 State table

| State | Trigger | Background | Border | Value text | Placeholder | Notes |
|-------|---------|------------|--------|------------|-------------|--------|
| **Default (empty)** | Rest, no value | `var(--color-bg-subtle)` | `1px solid var(--color-border-input)` | — | `var(--color-text-secondary)` | Placeholder is hint only; meet contrast for large/non-critical use or use `text.muted` if spec allows. |
| **Default (filled)** | Rest, has value | `var(--color-bg-canvas)` | `1px solid var(--color-border-input)` | `var(--color-text-primary)` | — | Same as Figma `surface/input-rest`. |
| **Hover** | `:hover` (enabled) | `var(--color-bg-canvas)` | `1px solid var(--color-border-input)` | inherits | inherits | No hover when disabled. |
| **Active** | `:active` (optional) | `var(--color-bg-canvas)` | `1px solid var(--color-primary)` | inherits | inherits | Optional; keep **1px** width; may match focus border color. |
| **Focus visible** | `:focus-visible` | `var(--color-bg-canvas)` | `1px solid var(--color-primary)` | inherits | inherits | **Same 1px width** as rest; add `outline` / focus-ring tokens (§7). Do not thicken border. |
| **Disabled** | `disabled` / `aria-disabled` | `var(--color-bg-subtle)` | `1px solid var(--color-border-disabled)` | `var(--color-text-muted)` | `var(--color-text-muted)` | `cursor: not-allowed`; optional `opacity` if token-aligned. |
| **Error** | `aria-invalid="true"` or invalid state | `var(--color-danger-muted)` | `1px solid var(--color-danger)` | `var(--color-text-primary)` | `var(--color-text-secondary)` | Show error row (§2). |
| **Success** | Valid / confirmed (product-defined) | `var(--color-success-muted)` | `1px solid var(--color-success)` | `var(--color-text-primary)` | — | Optional; use when design requires inline success. |

**Label colors (disabled)** — Label text: `var(--color-text-muted)`; required asterisk when disabled: same or `var(--color-text-muted)` (Figma maps disabled label to muted palette).

---

## 7. Design tokens reference (unified)

Map Figma variables to semantic tokens and typical CSS variables. Implementations should define aliases (e.g. `--color-border-input`) mapped from primitives or semantic roles.

| Role | Semantic token | Typical CSS variable | Figma variable (reference only) |
|------|----------------|----------------------|----------------------------------|
| Text / value | semantic.text.primary | `--color-text-primary` | text/primary |
| Placeholder / helper tone | semantic.text.secondary | `--color-text-secondary` | reference/helper |
| Placeholder (minimal) | semantic.text.muted | `--color-text-muted` | (when secondary too strong) |
| Muted / disabled text | semantic.text.muted | `--color-text-muted` | text/disabled |
| Input background (empty) | semantic.background.subtle | `--color-bg-subtle` | surface/input |
| Input background (filled, hover, focus) | semantic.background.canvas | `--color-bg-canvas` | surface/input-rest, surface/input-hover, surface/input-focus |
| Neutral input border | semantic.secondary.default | `--color-border-input` | border/input |
| Focus / active border accent | semantic.primary.default | `--color-primary` | border/focus |
| Disabled border | *(primitive gray.400)* | `--color-border-disabled` | border/disabled |
| Error border / message / asterisk | semantic.danger.default | `--color-danger` | reference/error, border/input-error |
| Error surface | semantic.danger.muted | `--color-danger-muted` | reference/error-weak |
| Success border / message | semantic.success.default | `--color-success` | reference/success, border/input-success |
| Success surface | semantic.success.muted | `--color-success-muted` | reference/success-weak |
| Focus ring (outline pattern) | semantic.primary.default | `--focus-ring-width`, `--focus-ring-color` | (align with button.md) |

**Gap — border alias tokens:** `color-semantic.json` does not define dedicated `border.input` / `border.disabled` roles. Use `--color-border-input` mapped from `semantic.secondary.default` (slate-neutral) and `--color-border-disabled` from `primitive.gray.400` until semantic border tokens are added.

---

## 8. Validation row (error / success)

Below the control when `state=error` or `state=success`:

| Element | Rule |
|---------|------|
| Layout | Horizontal row: icon + message; `gap: var(--space-sm)`; align center. |
| Icon | **Material** error/success filled (or closest match), **16px** visual size; decorative if message text is redundant, else `aria-hidden="true"` when message is exposed as text. |
| Error text | `font-size: var(--text-sm); line-height: var(--line-height-body-m); color: var(--color-danger)`. |
| Success text | `font-size: var(--text-sm); line-height: var(--line-height-body-m); color: var(--color-success)`. |

Associate the message with the input: `aria-describedby` on the control pointing to the message `id`. For errors, set `aria-invalid="true"`.

---

## 9. Accessibility

- **Label**: Visible label associated with `for`/`id` or wrapping control. Required fields: indicate required (asterisk + text in label or `aria-required="true"`).
- **aria-describedby**: Space-separated ids for hint, error, and/or success message as applicable.
- **Contrast**: WCAG 2.2 AA for label, value, placeholder, borders, and messages on their backgrounds (`policies/accessibility.md`).
- **Keyboard**: Native focus order; trailing icon buttons focusable and activatable with Enter/Space.
- **Focus**: Visible `:focus-visible` indicator: **outline** or ring ≥ **2px** equivalent per global policy; keep **input border at 1px** so focus does not resize the control (see §6.1).

See `intents/form-submission.md` for validation timing (submit / blur; avoid noisy per-keystroke validation for free text).

---

## 10. Motion

Optional short transition on border/background (`motion.duration.normal`, `motion.ease` — same token family as `button.md`). Respect `prefers-reduced-motion: reduce` (`policies/motion.md`).

---

## 11. Implementation hooks (example)

### CSS classes (BEM-style)

| Class | Purpose |
|-------|---------|
| `ds-field` | Vertical stack (label + control + messages). |
| `ds-field__label` | Label row (includes optional required asterisk). |
| `ds-input` | Control surface (border, radius, padding) or native element when styles applied directly. |
| `ds-input--error` | Error border/background. |
| `ds-input--success` | Success border/background. |
| `ds-input--disabled` | Disabled visuals (or use `:disabled`). |
| `ds-field__hint` | Helper text. |
| `ds-field__message--error` | Error message row. |
| `ds-field__message--success` | Success message row. |

### Example CSS (token-correct properties)

```css
.ds-field {
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  align-items: stretch;
}
.ds-field__label {
  font-family: var(--font-sans);
  font-size: var(--text-sm);
  font-weight: 600;
  line-height: var(--line-height-body-m);
  color: var(--color-text-primary);
}
.ds-input {
  box-sizing: border-box;
  min-height: 2.5rem;
  padding: var(--space-sm) var(--space-sm) var(--space-sm) var(--space-md);
  border: 1px solid var(--color-border-input);
  border-radius: var(--radius-sm);
  background: var(--color-bg-subtle);
  font-family: var(--font-sans);
  font-size: var(--text-sm);
  font-weight: 400;
  line-height: var(--line-height-body-m);
  color: var(--color-text-primary);
}
.ds-input::placeholder {
  color: var(--color-text-secondary);
}
.ds-input:focus-visible {
  /* Keep border-width 1px — do not thicken here (layout shift). */
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
  border-color: var(--color-primary);
  background: var(--color-bg-canvas);
}
```

### Props (React-style reference)

| Prop | Type | Default |
|------|------|---------|
| `invalid` | `boolean` | `false` |
| `success` | `boolean` | `false` |
| `disabled` | `boolean` | `false` |
| `required` | `boolean` | `false` |
| `hint` | `string` | — |
| `size` | `'small' \| 'medium'` | `'medium'` |

---

## 12. Do's and Don'ts

- **Do** use semantic colors and typography tokens; set `font-size`, `line-height`, and `color` with the correct CSS properties.
- **Do** use native `<input>` / `<textarea>` and associated label/error semantics.
- **Do** keep **1px** border width for all states; use **outline** or **box-shadow** for focus emphasis so the field does not resize on focus (§6.1).
- **Don't** use Tailwind `text-[var(--text-sm)]` thinking it sets font-size — it sets **color**.
- **Don't** increase `border-width` on `:focus-visible` or between rest and focus — causes shrink/grow.
- **Don't** validate on every keystroke for long free-text fields unless product requires it.

---

## 13. Example usage (AI-readable)

On a profile form, add an **Email** field: visible label "Email" linked with `for`/`id`, `type="email"`, placeholder "you@example.com" using placeholder color token, optional hint "We'll never share your email" with `aria-describedby`. On blur or submit, if invalid, render "Enter a valid email" in `ds-field__message--error`, set `aria-invalid="true"` on the input, and include the error element id in `aria-describedby`. Use `gap: var(--space-xs)` between label and input and `var(--space-md)` between this field and the next. For a **success** state after save, optionally show `ds-field__message--success` with success text color token and link via `aria-describedby` if it conveys field-level status.

---

*Spec version: 2.1 — 1px borders; stable layout on focus (no border-width change); token-mapped for codegen.*
