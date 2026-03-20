# Button — Complete Component Spec

<!-- Design system component: buttons. AI-readable. Use design-system tokens only. Reference: accessibility.md, iconography.md, modal.md (for destructive confirmation). -->

---

## Meta

```yaml
document_type: component_spec
component_id: button
scope: actions | forms | navigation_trigger
audience: ai_consumers | developers | designers
version: 2.0
token_sources:
  - design-system/tokens/color-semantic.json
  - design-system/tokens/typography.json
  - design-system/tokens/spacing.json
```

---

## 1. Purpose

Buttons trigger a single action or navigate the user. Use them for primary, secondary, destructive, and low-emphasis (ghost) actions.

**When to use**: Form submit/cancel, toolbars, cards, modals, table row actions, and any UI that needs a clear call-to-action or alternative action (Cancel, Back, Skip).

**When not to use**: Use a link (`<a href>`) for navigation to a URL; use a button for in-page or programmatic actions. Do not use `<div role="button">` or clickable `<span>`; use `<button>`.

---

## 2. Structure

| Part | Required | Description |
|------|----------|-------------|
| **Label** | Yes (or icon-only) | Visible text (e.g. "Save", "Cancel") or icon with an accessible name. |
| **Icon** | Optional | Leading or trailing; icon-only buttons **must** have `aria-label`. |
| **Loading indicator** | Optional | Spinner when `loading` is true; do not rely on motion alone (respect `prefers-reduced-motion`). |

- Use a single `<button>` element (or `<a>` only when styled as button for navigation and `href` is present).
- No wrapper div around the trigger.

---

## 3. Variants

| Variant | When to use | Confirmation |
|---------|-------------|--------------|
| **Primary** | Main action in the current context (Save, Continue, Submit). One per section or view. | — |
| **Secondary** | Alternative actions (Cancel, Back, Skip). Outline or muted fill. | — |
| **Destructive** | Delete, remove, or irreversible reset. | **Must** open a confirmation modal before executing (see `components/modal.md`). |
| **Ghost** | Low emphasis (Dismiss, Skip). Transparent with hover state. | — |
| **Icon-only** | Single icon (close, edit, delete, menu). | **Must** have `aria-label` describing the action. |

---

## 4. Sizes (unified)

All variants support the same size scale. Values use design-system tokens.

| Size | Height | Horizontal padding | Label typography | Line-height |
|------|--------|---------------------|------------------|-------------|
| **Small** | 1.5rem (24px) | `var(--text-xs)` (0.75rem) | `var(--text-xs)`, `font-weight: 600` | tight |
| **Medium** | 2rem (32px) | `var(--text-sm)` (0.875rem) | `var(--text-sm)`, `font-weight: 600` | tight |
| **Large** | 2.5rem (40px) | `var(--text-base)` (1rem) | `var(--text-base)`, `font-weight: 600` | snug |

- **Font family**: `var(--font-sans)` for the label.
- **Vertical padding**: Implied by min-height; label vertically centered.
- Default size for most contexts: **Medium**.

### Lenovo Zero-Floor theme (`data-theme="lenovo"`)

When implementing buttons for the Lenovo brand layer, read [`design-system/tokens/lenovo-zero-floor.md`](../tokens/lenovo-zero-floor.md) and [`color-semantic-lenovo.json`](../tokens/color-semantic-lenovo.json). **Label weight:** use **`font-weight: 500`** (medium), not `600` — Lenovo disallows semibold/600. Use semantic interactive tokens (`semantic.interactive.primary*`, `semantic.feedback.error` for destructive) and CSS variables from `design-system/css/design-tokens-lenovo.css`; primary fill is Lenovo purple, not red.

---

## 5. Interaction states (unified)

| State | Trigger | Behavior |
|-------|---------|----------|
| **Default** | Rest | Visual per variant (see §7). |
| **Hover** | `:hover` | Distinct change (background, border, or opacity). Not applied when disabled or loading. |
| **Focus** | `:focus-visible` | Visible focus ring (min 2px per accessibility policy). Never style `:focus` only. |
| **Active (pressed)** | `:active` | Optional subtle scale (e.g. `scale(0.98)`). Respect `prefers-reduced-motion`. |
| **Disabled** | `disabled` or `aria-disabled="true"` | Non-interactive; `cursor: not-allowed`; `pointer-events: none`; muted colors. |
| **Loading** | `data-loading="true"` / `aria-busy="true"` | Same look as disabled + trailing spinner; label remains visible; prevent click. |

---

## 6. Design tokens reference (unified)

Map to your CSS variables or theme. Values come from `color-semantic.json` and `typography.json`.

| Role | Semantic token | Typical CSS variable |
|------|----------------|----------------------|
| Primary default | semantic.primary.default | `--color-primary` |
| Primary hover | semantic.primary.hover | `--color-primary-hover` |
| Primary on (text/icon) | semantic.primary.onPrimary | `--color-primary-on` |
| Primary muted (spinner track) | semantic.primary.muted | `--color-primary-muted` |
| Secondary default bg | semantic.secondary.muted | `--color-secondary-muted` |
| Secondary hover bg | (darker mute) | `--color-secondary-muted-hover` or primitive |
| Secondary border | semantic.secondary.default | `--color-secondary` |
| Secondary hover border | semantic.secondary.default | `--color-secondary` |
| Destructive default | semantic.danger.default | `--color-danger` |
| Destructive hover | semantic.danger.hover | `--color-danger-hover` |
| Destructive on | semantic.danger.onDanger | `--color-danger-on` |
| Ghost default | transparent | — |
| Ghost hover bg | semantic.background.subtle | `--color-bg-subtle` |
| Text primary (secondary/ghost label) | semantic.text.primary | `--color-text-primary` |
| Disabled bg | semantic.background.subtle | `--color-bg-subtle` |
| Disabled text | semantic.text.muted | `--color-text-muted` |
| Focus ring | (primary or dedicated) | `--focus-ring-width`, `--focus-ring-color` |
| Border radius pill | — | `9999px` or `50%` (Primary, Secondary, Destructive) |
| Border radius rounded | radius.md | `var(--radius-md)` (Ghost) |
| Spacing gap (icon–label) | space.sm | `var(--space-sm)` |
| Motion | motion.duration.normal, motion.ease | `var(--motion-duration-normal)`, `var(--motion-ease)` |

---

## 7. Variant specifications

### 7.1 Primary

| Property | Value |
|---------|--------|
| Shape | Pill (fully rounded) |
| Border radius | `9999px` or `50%` |
| Border | None |
| Font weight | 600 (semibold) |

#### States (Primary)

| State | Background | Text/icon | Border | Cursor | Notes |
|-------|------------|-----------|--------|--------|-------|
| Default | `--color-primary` | `--color-primary-on` | none | pointer | — |
| Hover | `--color-primary-hover` | `--color-primary-on` | none | pointer | — |
| Active | `--color-primary-hover` | `--color-primary-on` | none | pointer | Optional `transform: scale(0.98)` |
| Focus | `--color-primary` | `--color-primary-on` | none | pointer | Focus ring 2px, offset 2px |
| Disabled | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | pointer-events: none |
| Loading | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | Trailing spinner; pointer-events: none |

**Padding (Primary)**: Vertical `0.75rem`; horizontal `var(--space-lg)`.

```css
.ds-btn--primary {
  background: var(--color-primary);
  color: var(--color-primary-on);
  border: none;
  border-radius: 9999px;
  padding: 0.75rem var(--space-lg);
  font-weight: 600;
  font-size: var(--text-base); /* large */ or var(--text-sm); /* medium */ or var(--text-xs); /* small */
  cursor: pointer;
}
.ds-btn--primary:hover:not(:disabled):not([data-loading="true"]) {
  background: var(--color-primary-hover);
}
.ds-btn--primary:active:not(:disabled):not([data-loading="true"]) {
  background: var(--color-primary-hover);
  transform: scale(0.98);
}
.ds-btn--primary:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
}
.ds-btn--primary:disabled,
.ds-btn--primary[aria-disabled="true"],
.ds-btn--primary[data-loading="true"] {
  background: var(--color-bg-subtle);
  color: var(--color-text-muted);
  cursor: not-allowed;
  pointer-events: none;
}
```

---

### 7.2 Secondary

| Property | Value |
|---------|--------|
| Shape | Pill |
| Border radius | `9999px` or `50%` |
| Border | 1px solid (or 2px per design) `--color-secondary` |
| Font weight | 600 |

#### States (Secondary)

| State | Background | Text/icon | Border | Cursor | Notes |
|-------|------------|-----------|--------|--------|-------|
| Default | `--color-secondary-muted` | `--color-text-primary` | 1px solid `--color-secondary` | pointer | — |
| Hover | `--color-secondary-muted-hover` (or darker mute) | `--color-text-primary` | 1px solid `--color-secondary` | pointer | — |
| Active | `--color-secondary-muted` | `--color-text-primary` | 1px solid `--color-secondary` | pointer | Optional scale(0.98) |
| Focus | `--color-secondary-muted` | `--color-text-primary` | 1px solid `--color-secondary` | pointer | Focus ring outside |
| Disabled | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | pointer-events: none |
| Loading | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | Trailing spinner |

**Padding (Secondary)**: Vertical from size (1.5rem / 2rem / 2.5rem min-height for small / medium / large); horizontal `var(--space-md)` or `var(--space-lg)`.

```css
.ds-btn--secondary {
  background: var(--color-secondary-muted);
  color: var(--color-text-primary);
  border: 1px solid var(--color-secondary);
  border-radius: 9999px;
  font-weight: 600;
  cursor: pointer;
}
.ds-btn--secondary:hover:not(:disabled):not([data-loading="true"]) {
  background: var(--color-secondary-muted-hover);
  border-color: var(--color-secondary);
}
.ds-btn--secondary:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
}
.ds-btn--secondary:disabled,
.ds-btn--secondary[data-loading="true"] {
  background: var(--color-bg-subtle);
  color: var(--color-text-muted);
  border-color: transparent;
  cursor: not-allowed;
  pointer-events: none;
}
```

---

### 7.3 Destructive

| Property | Value |
|---------|--------|
| Shape | Pill |
| Border radius | `9999px` or `50%` |
| Border | None |
| Font weight | 600 |

**Requirement**: Destructive buttons must **not** execute the action on a single click. They must open a confirmation modal (see `components/modal.md`) with a clear title and description; the primary action in the modal performs the destructive operation.

#### States (Destructive)

| State | Background | Text/icon | Border | Cursor | Notes |
|-------|------------|-----------|--------|--------|-------|
| Default | `--color-danger` | `--color-danger-on` | none | pointer | — |
| Hover | `--color-danger-hover` | `--color-danger-on` | none | pointer | — |
| Active | `--color-danger-hover` | `--color-danger-on` | none | pointer | Optional scale(0.98) |
| Focus | `--color-danger` | `--color-danger-on` | none | pointer | Focus ring 2px, offset 2px |
| Disabled | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | pointer-events: none |
| Loading | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | Trailing spinner |

**Padding (Destructive)**: Same as Primary (0.75rem vertical, `var(--space-lg)` horizontal).

```css
.ds-btn--destructive {
  background: var(--color-danger);
  color: var(--color-danger-on);
  border: none;
  border-radius: 9999px;
  padding: 0.75rem var(--space-lg);
  font-weight: 600;
  cursor: pointer;
}
.ds-btn--destructive:hover:not(:disabled):not([data-loading="true"]) {
  background: var(--color-danger-hover);
}
.ds-btn--destructive:active:not(:disabled):not([data-loading="true"]) {
  background: var(--color-danger-hover);
  transform: scale(0.98);
}
.ds-btn--destructive:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
}
.ds-btn--destructive:disabled,
.ds-btn--destructive[data-loading="true"] {
  background: var(--color-bg-subtle);
  color: var(--color-text-muted);
  cursor: not-allowed;
  pointer-events: none;
}
```

---

### 7.4 Ghost

| Property | Value |
|---------|--------|
| Shape | Rounded (not pill) |
| Border radius | `var(--radius-md)` |
| Border | None (or very subtle) |
| Font weight | 600 |

#### States (Ghost)

| State | Background | Text/icon | Border | Cursor | Notes |
|-------|------------|-----------|--------|--------|-------|
| Default | transparent | `--color-text-primary` | none | pointer | — |
| Hover | `--color-bg-subtle` | `--color-text-primary` | none | pointer | — |
| Active | `--color-bg-subtle` | `--color-text-primary` | none | pointer | Optional scale(0.98) |
| Focus | transparent | `--color-text-primary` | none | pointer | Focus ring 2px, offset 2px |
| Disabled | transparent | `--color-text-muted` | none | not-allowed | pointer-events: none |
| Loading | `--color-bg-subtle` | `--color-text-muted` | none | not-allowed | Trailing spinner |

**Padding (Ghost)**: Same horizontal/vertical as other variants for the chosen size; often `var(--space-md)` for icon-only.

```css
.ds-btn--ghost {
  background: transparent;
  color: var(--color-text-primary);
  border: none;
  border-radius: var(--radius-md);
  font-weight: 600;
  cursor: pointer;
}
.ds-btn--ghost:hover:not(:disabled):not([data-loading="true"]) {
  background: var(--color-bg-subtle);
}
.ds-btn--ghost:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
}
.ds-btn--ghost:disabled,
.ds-btn--ghost[data-loading="true"] {
  background: var(--color-bg-subtle);
  color: var(--color-text-muted);
  cursor: not-allowed;
  pointer-events: none;
}
```

---

## 8. Icon and icon-only

- **Icon set**: Per `design-system/policies/iconography.md` — Material icons only (e.g. Material Symbols).
- **Size**: Default 24dp; compact 20dp for medium button; 18dp when space is tight.
- **Gap**: Between icon and label: `var(--space-sm)`. Edge-to-icon padding matches button horizontal padding.
- **Color**: Same as label (variant’s text token). Decorative icon that duplicates label: `aria-hidden="true"`. Icon-only: **must** have `aria-label` (no visible text).
- **Icon-only button**: Same variants and sizes; use square or consistent padding (e.g. `var(--space-md)`) so touch target is at least 44×44px when possible.

---

## 9. Loading state (all variants)

- **Trigger**: `data-loading="true"` and/or `aria-busy="true"`.
- **Visual**: Same as disabled (muted background and text) plus a **trailing** spinner after the label. Label remains visible (e.g. "Saving…" or "Submit" with spinner).
- **Spinner**: Circular, ~1rem (16px); border 2px; track `var(--color-primary-muted)` or `var(--color-border)`; accent `var(--color-primary)` (or variant-appropriate). Animation: rotate 360deg, ~0.75s linear infinite. Under `prefers-reduced-motion: reduce`, set animation to none or show static state.
- **Behavior**: Disable the button (or treat as disabled) to prevent double submit. Expose loading to assistive tech via `aria-busy="true"`; optionally append " loading" to the accessible name.

```css
.ds-btn[data-loading="true"] {
  background: var(--color-bg-subtle) !important;
  color: var(--color-text-muted) !important;
  cursor: not-allowed;
  pointer-events: none;
  display: inline-flex;
  align-items: center;
  gap: var(--space-sm);
}
.ds-btn-loading-label { /* keep label visible */ }
.ds-btn-spinner {
  width: 1rem;
  height: 1rem;
  border: 2px solid var(--color-primary-muted);
  border-top-color: var(--color-primary);
  border-radius: 50%;
  animation: ds-btn-spin 0.75s linear infinite;
}
@media (prefers-reduced-motion: reduce) {
  .ds-btn-spinner { animation: none; }
}
```

---

## 10. API / Implementation

### 10.1 HTML

- **Element**: `<button type="button">` or `type="submit"` when inside a form and is the submit trigger. Do not use `<div role="button">` or `<span>` with click handlers.
- **Attributes**:
  - `disabled` or `aria-disabled="true"` when not interactive.
  - `aria-busy="true"` when loading.
  - `aria-label` when icon-only or when visible label is not sufficient (e.g. "Close", "Delete item").
  - `data-loading="true"` for loading styling (optional if `aria-busy` is sufficient for your CSS).

### 10.2 Class names (BEM-style)

| Class | Purpose |
|-------|---------|
| `ds-btn` | Base: layout, transitions, focus, disabled, loading |
| `ds-btn--primary` | Primary variant |
| `ds-btn--secondary` | Secondary variant |
| `ds-btn--destructive` | Destructive variant |
| `ds-btn--ghost` | Ghost variant |
| `ds-btn--small` | Small size (24px height) |
| `ds-btn--medium` | Medium size (default) |
| `ds-btn--large` | Large size |
| `ds-btn-loading-label` | Wrapper for label when loading (keeps it visible) |
| `ds-btn-spinner` | Loading spinner element; use `aria-hidden` on spinner |

Example: `<button class="ds-btn ds-btn--primary ds-btn--medium" type="submit">Save</button>`.

### 10.3 React (example contract)

```ts
interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'destructive' | 'ghost'
  size?: 'small' | 'medium' | 'large'
  loading?: boolean
}
```

- **variant**: Default `'secondary'` if not specified.
- **size**: `'small'` | `'medium'` | `'large'`. Default `'medium'`.
- **loading**: When `true`, render spinner, set `aria-busy="true"`, and treat as disabled for click (e.g. `disabled={disabled || loading}`).
- Forward ref to the underlying `<button>`. Spread remaining props (e.g. `type`, `onClick`, `aria-label`).

---

## 11. Accessibility

- **Accessible name**: Every button has an accessible name (visible label or `aria-label`). Icon-only buttons **must** have `aria-label`.
- **Contrast**: Label and icon meet **≥ 4.5:1** contrast on the button background (WCAG 2.2 AA).
- **Keyboard**: Focusable by default; activatable with **Enter** and **Space**.
- **Focus ring**: Visible on `:focus-visible` only; minimum width per accessibility policy (e.g. 2px); sufficient contrast.
- **Disabled**: Use `disabled` or `aria-disabled="true"` so assistive tech can announce the state.
- **Loading**: Use `aria-busy="true"`; optionally include "loading" in the accessible name when loading.
- **Destructive**: Never execute on single click without a confirmation modal with title and description (see `components/modal.md`).

---

## 12. Motion

- **Hover / active**: Use design-system motion tokens (e.g. 150ms ease) for background, border, color, opacity, transform. Respect `prefers-reduced-motion: reduce` (e.g. no scale on active when reduced).
- **Loading spinner**: Rotation animation; under `prefers-reduced-motion: reduce`, disable animation or show static state.
- Keep transitions short (150–200ms); avoid decorative motion on the button itself.

---

## 13. Do's & Don'ts

| Do | Don't |
|----|--------|
| Use one primary button per context. | Use multiple primary buttons in the same section. |
| Pair destructive with a confirmation modal. | Execute destructive action on a single click without confirmation. |
| Provide `aria-label` for every icon-only button. | Rely on icon alone without an accessible name. |
| Set `aria-busy="true"` and optionally update accessible name when loading. | Use `<div role="button">` or clickable `<span>`. |
| Use `<button type="submit">` for form submit. | Use destructive variant for reversible or low-risk actions. |
| Use design-system tokens only; no hardcoded colors or spacing. | Rely on color alone to convey state; combine with cursor/icon/text. |

---

## 14. Example usage (AI-readable)

- **Modal footer**: Primary "Save Changes" at bottom right; secondary "Cancel" to its left.
- **Form**: Primary "Submit" and secondary "Cancel" at end of form or in sticky footer.
- **Table row**: Ghost or icon-only "Delete" that opens modal "Delete device?" with description "This action cannot be undone." and Confirm (destructive) / Cancel (secondary) in modal.
- **Icon-only**: `<button type="button" class="ds-btn ds-btn--ghost ds-btn--medium" aria-label="Close">` with only an icon (e.g. close) inside.
- **Loading**: Same button with `data-loading="true"` and `aria-busy="true"`; label "Submit" plus trailing spinner; button not clickable.

---

*Spec version: 2.0 — Complete spec with all variants, API, and token reference.*
