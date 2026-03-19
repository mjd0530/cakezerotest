# Toggle — Complete Component Spec

<!-- Design system component: switch / boolean control. AI-readable. Use design-system tokens only; map each spec property to the correct CSS property (see policies/tokens-and-css.md). Reference: accessibility.md, policies/typography.md, policies/motion.md. -->

---

## Meta

```yaml
document_type: component_spec
component_id: toggle
scope: settings | preferences | forms_boolean
audience: ai_consumers | developers | designers
version: 2.0
figma_source: MCP get_design_context on selected Toggle component (states Enabled/Hovered/Pressed/Focus/Disabled × on/off)
token_sources:
  - design-system/tokens/color-semantic.json
  - design-system/tokens/color-semantic-dark.json
  - design-system/tokens/color-primitives.json
  - design-system/tokens/typography.json
  - design-system/tokens/spacing.json
```

---

## 1. Purpose

Toggles set a **single on/off (boolean)** value without a separate submit step (e.g. “Enable notifications”, “Dark mode”, “Public profile”). Use a toggle when the choice is binary and the result can apply immediately or on save.

**When to use**: Settings, feature flags, preferences, and any single yes/no control. Prefer a toggle over two radio buttons for boolean settings.

**When not to use**: Use a **checkbox** when selecting one or more items from a set or agreeing to a single legal statement. Do not use a toggle for non-boolean multi-state selection.

---

## 2. Structure

| Part | Required | Description |
|------|----------|-------------|
| **Label** | Yes (unless `aria-label` with design approval) | Visible text for the setting; typically **left** of the switch in LTR. Associate via `for`/`id`, wrapping, or `aria-labelledby`. |
| **Switch** | Yes | **Track** (pill background) + **thumb** (movable indicator). Implement as `<button type="button" role="switch">` with `aria-checked`, or native `<input type="checkbox" role="switch">` with correct styling. |
| **Hint** | Optional | Helper text; link with `aria-describedby` on the switch. |

- **Layout (LTR):** Horizontal row — label, then `gap`, then switch. Mirror for RTL (`flex-direction: row-reverse` or logical properties).

---

## 3. Variants

| Variant | When to use |
|---------|-------------|
| **Default** | Label + switch in one row. |
| **With hint** | Helper below label or row; hint `id` on `aria-describedby` of the switch. |

---

## 4. Dimensions and typography (default size)

Single default size from Figma; use **rem** and tokens — no magic numbers in implementation.

| Element | Size | Notes |
|---------|------|--------|
| **Track** | `width: 2.5rem` (40px), `height: 1.5rem` (24px) | Pill: `border-radius: 9999px` (or `50%` height). |
| **Thumb** | `width: 1rem`, `height: 1rem` (16px) | Circle: `border-radius: 50%`. Vertically centered in track (`top: 50%` + translate or flex). |
| **Thumb position (off)** | Inset `var(--space-xs)` (4px) from **start** edge (LTR: `left`). | |
| **Thumb position (on)** | Inset `var(--space-xs)` from **end** edge (LTR: `right: var(--space-xs)` or `left: calc(2.5rem - 1rem - var(--space-xs))`). | Use logical properties (`inset-inline`) for RTL. |
| **Label** | `font-size: var(--text-sm); font-weight: 600; line-height: var(--line-height-body-m); font-family: var(--font-sans); color: var(--color-text-primary)` | Same weight/size family as compact form labels (see `input.md` label). |

**Gap (label ↔ switch):** `gap: var(--space-lg)` (16px in default spacing scale).

**Layout stability:** Do not change **track outer width/height** between states. **Focus** must use **outline** or outer **box-shadow** ring (see §5.2), not a thicker border that resizes the track (same principle as `input.md` §6.1).

---

## 5. Interaction states — switch surface

### 5.1 Visual states (rest / hover / pressed)

| State | `aria-checked` | Track background | Thumb background | Notes |
|-------|----------------|------------------|------------------|--------|
| **Off — default** | `false` | `--color-toggle-track-off` | `--color-toggle-thumb-off` | Map track to `primitive.slate.700`; thumb to `semantic.background.canvas` (see §7). |
| **Off — hover** | `false` | `--color-toggle-track-off-hover` | `--color-toggle-thumb-off` | Map track to `primitive.slate.900` (Figma: dark hover). |
| **Off — pressed** | `false` | `--color-toggle-track-off` | `--color-toggle-thumb-off-pressed` | Figma: thumb may **widen** to ~24px (`1.5rem`) toward the on direction; optional affordance. |
| **On — default** | `true` | `var(--color-primary)` | `var(--color-primary-on)` | `semantic.primary.default` + `semantic.primary.onPrimary`. |
| **On — hover** | `true` | `--color-toggle-track-on-hover` | `var(--color-primary-on)` | Map to `primitive.blue.800` or `semantic.primary.hover` per theme. |
| **On — pressed** | `true` | `var(--color-primary)` | `--color-toggle-thumb-on-pressed` | Figma: thumb may widen with **surface** fill (`semantic.background.canvas`); optional. |
| **Disabled — off** | `false` | `--color-toggle-track-disabled` | `--color-toggle-thumb-disabled` | Track: `primitive.gray.200`; thumb: `primitive.slate.500`. |
| **Disabled — on** | `true` | Same disabled track | Same disabled thumb | Thumb position remains **on** (end side). |

**Disabled label:** `color: var(--color-text-muted)`.

### 5.2 Focus (`:focus-visible`)

- **Do not** increase track border width in a way that shifts layout.
- Use **`outline`** (min 2px per global accessibility policy) + `outline-offset`, or a dedicated focus ring layer **outside** the track (Figma: ~`44×28px` ring around `40×24` track, `2px` **border** in **primary** color). Prefer **`outline: var(--focus-ring-width) solid var(--focus-ring-color)`** with offset so the hit area stays **2.5rem × 1.5rem**.

---

## 6. Design tokens reference

Map Figma roles to semantic primitives and **typical CSS variables**. Implementations define aliases where no semantic key exists yet.

| Role | Semantic / primitive | Typical CSS variable |
|------|------------------------|----------------------|
| Track off (rest) | `primitive.slate.700` | `--color-toggle-track-off` |
| Track off (hover) | `primitive.slate.900` | `--color-toggle-track-off-hover` |
| Track on | `semantic.primary.default` | `--color-primary` |
| Track on (hover) | `primitive.blue.800` or `semantic.primary.hover` | `--color-toggle-track-on-hover` |
| Thumb on primary fill | `semantic.primary.onPrimary` | `--color-primary-on` |
| Thumb off (on dark track) | `semantic.background.canvas` | `--color-toggle-thumb-off` |
| Disabled track | `primitive.gray.200` | `--color-toggle-track-disabled` |
| Disabled thumb | `primitive.slate.500` | `--color-toggle-thumb-disabled` |
| Label (enabled) | `semantic.text.primary` | `--color-text-primary` |
| Label (disabled) | `semantic.text.muted` | `--color-text-muted` |
| Focus ring | `semantic.primary.default` | `--focus-ring-width`, `--focus-ring-color` |

**Gap — toggle-specific semantics:** `color-semantic.json` does not yet define `toggle.trackOff` etc. Use the aliases above mapped from primitives/primary until semantic toggle roles are added.

**Dark theme:** For dark UI, resolve the same variable **names** from [`color-semantic-dark.json`](color-semantic-dark.json) and tuned primitives (e.g. lighter track/thumb steps); keep **contrast** between thumb and track ≥ **3:1** (non-text UI) and label text ≥ **4.5:1** on page background.

---

## 7. Accessibility

- **Role:** `role="switch"` with `aria-checked="true"` | `"false"`.
- **Name:** Visible label associated with the switch, or `aria-labelledby` / `aria-label`.
- **Keyboard:** Focusable; **Space** toggles (and **Enter** where platform expects it). Do not trap focus.
- **State:** Screen readers announce on/off (from `aria-checked` and label).
- **Disabled:** `aria-disabled="true"` or `disabled`; remove from tab order or use `tabindex="-1"` per pattern; label remains readable.

See `.cursor/rules/accessibility.mdc` and `policies/accessibility.md`.

---

## 8. Motion

- Thumb may **slide** between off/on (`motion.duration.normal`, `motion.ease` per `policies/motion.md`).
- **`prefers-reduced-motion: reduce`:** Prefer **instant** thumb position change or minimal duration.

---

## 9. Implementation hooks (example)

### CSS classes (BEM-style)

| Class | Purpose |
|-------|---------|
| `ds-toggle` | Row: label + switch. |
| `ds-toggle__label` | Label text. |
| `ds-toggle__switch` | Focusable control wrapping track + thumb (or single button styling track). |
| `ds-toggle__track` | Pill track. |
| `ds-toggle__thumb` | Thumb element. |
| `is-on` / `aria-checked` | Style hook for on state. |
| `is-disabled` | Disabled visuals. |

### Example CSS (token-correct; LTR)

```css
.ds-toggle {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: var(--space-lg);
}
.ds-toggle__label {
  font-family: var(--font-sans);
  font-size: var(--text-sm);
  font-weight: 600;
  line-height: var(--line-height-body-m);
  color: var(--color-text-primary);
}
.ds-toggle__switch {
  position: relative;
  width: 2.5rem;
  height: 1.5rem;
  padding: 0;
  border: none;
  background: transparent;
  cursor: pointer;
  border-radius: 9999px;
  flex-shrink: 0;
}
.ds-toggle__switch:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: 2px;
}
.ds-toggle__track {
  position: absolute;
  inset: 0;
  border-radius: 9999px;
  background: var(--color-toggle-track-off);
}
.ds-toggle__switch[aria-checked="true"] .ds-toggle__track {
  background: var(--color-primary);
}
.ds-toggle__thumb {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  background: var(--color-toggle-thumb-off);
  left: var(--space-xs);
}
.ds-toggle__switch[aria-checked="true"] .ds-toggle__thumb {
  background: var(--color-primary-on);
  left: auto;
  right: var(--space-xs);
}
.ds-toggle__switch:disabled .ds-toggle__track {
  background: var(--color-toggle-track-disabled);
  cursor: not-allowed;
}
.ds-toggle__switch:disabled .ds-toggle__thumb {
  background: var(--color-toggle-thumb-disabled);
}
.ds-toggle:has(.ds-toggle__switch:disabled) .ds-toggle__label {
  color: var(--color-text-muted);
}
```

Adjust hover/pressed rules in implementation to match §5.1; keep **track box size** fixed.

### Props (React-style reference)

| Prop | Type | Default |
|------|------|---------|
| `checked` | `boolean` | `false` |
| `disabled` | `boolean` | `false` |
| `onChange` | `(next: boolean) => void` | — |
| `id` / `name` | `string` | — |

---

## 10. Do's and Don'ts

- **Do** use semantic colors and spacing tokens; keep **focus** on `:focus-visible` without resizing the track.
- **Do** expose on/off to assistive tech with `aria-checked`.
- **Don't** use a plain `<div>` click target without `role="switch"` and keyboard support.
- **Don't** use a toggle for multi-select lists; use checkboxes.

---

## 11. Example usage (AI-readable)

On a **Settings → Notifications** section, add a row: visible label **“Email notifications”** to the left, switch to the right, `gap: var(--space-lg)`. The switch is a `<button type="button" role="switch" aria-checked="false">` (or controlled `aria-checked`) with accessible name from the label. On activation, set `aria-checked` to `true` and persist the preference. Optional hint: “We’ll send account updates” with `aria-describedby`. On keyboard **Space**, toggle; show **`:focus-visible`** ring without changing track dimensions. When disabled, use disabled track/thumb tokens and `color: var(--color-text-muted)` on the label.

---

*Spec version: 2.0 — Complete spec; Figma via MCP; token-mapped for codegen.*
