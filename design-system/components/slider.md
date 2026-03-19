# Slider — complete component spec

<!-- Design system component: single-thumb and range sliders with optional value field. AI-readable. Use design-system tokens only; map each spec property to the correct CSS property (see policies/tokens-and-css.md). Reference: accessibility.md, policies/typography.md, policies/motion.md, components/input.md. -->

---

## Meta

```yaml
document_type: component_spec
component_id: slider
scope: forms | filters | settings | media
audience: ai_consumers | developers | designers
version: 1.0
figma_source: >-
  MCP get_design_context / get_variable_defs on Figma:
  frame "Basic" (24057:594), symbol State=Enabled (24057:593), State=Hovered (24057:592),
  State=Focused (24057:589), State=Disabled (24057:590);
  frame "Double" (37386:832), symbol State=Enabled (37386:833).
token_sources:
  - design-system/tokens/color-semantic.json
  - design-system/tokens/color-semantic-dark.json
  - design-system/tokens/color-primitives.json
  - design-system/tokens/typography.json
  - design-system/tokens/spacing.json
spacing_scale: space.xs | space.sm | space.md | space.lg | space.xl (see design-system/tokens/spacing.md)
radius: radius.sm (value field corners, optional value badge — 4px in Figma)
```

---

## 1. Purpose

Sliders adjust a **numeric value** along a continuous range by dragging a **thumb** on a **track**, optionally showing min/max labels and a **value field** (read-only or editable per product). Use for volume, percentages, price ranges, and coarse numeric tuning where precision typing is secondary.

**When to use**: Bounded numeric input when spatial control helps (e.g. 0–100%), or dual-thumb **range** selection (min/max).

**When not to use**: Discrete steps with few options — prefer **dropdown** or **radio** group. Boolean on/off — use **toggle** (`components/toggle.md`). Unbounded or exact numeric entry — prefer **input** `type="number"` (`components/input.md`).

---

## 2. Structure

### 2.1 Field layout (single thumb — `Basic`)

| Part | Required | Description |
|------|----------|-------------|
| **Label** | Yes (unless `aria-label` with design approval) | Above the control row; sentence case. Same typography as **input** label: `font-size: var(--text-sm); font-weight: 600; line-height: var(--line-height-body-m); font-family: var(--font-sans); color: var(--color-text-primary)`. |
| **Min label** | Optional (default in Figma) | Short text or number (e.g. `0`); `font-size: var(--text-sm); font-weight: 400; line-height: var(--line-height-body-m); color: var(--color-text-primary)`. |
| **Track** | Yes | Horizontal rail **4px** tall (`height: var(--space-xs)` at 16px root). Split into **filled** (min → thumb) and **unfilled** (thumb → max) regions; flex-grow segments with `gap: var(--space-sm)` between min label and track start, and between track end and max label. |
| **Thumb (handle)** | Yes | **Hit target** `48px × 48px` (3rem); visible thumb **~18px** diameter centered in hit target (Figma exports as nested “State layer” graphics). Implement as focusable control (see §8). |
| **Max label** | Optional | Same typography as min label. |
| **Value field** | Optional (`hasField` in Figma) | Narrow column **64px** min width, **40px** height, aligned with **input** medium value styling (`text-sm`, `body-m` line-height). Shows current value (e.g. `50%`); may be read-only or `type="number"`/`text` with validation per product. |

**Vertical stack:** `gap: var(--space-xs)` between label and row.

**Horizontal row (label + track + optional field):** `gap: var(--space-xl)` (Figma 24px) between the **track group** (flex `1 1 auto`) and the **value field**.

### 2.2 Range layout (`Double`)

| Part | Description |
|------|-------------|
| **Label** | Same as single. |
| **Leading value field** | Optional; shows **low** end of range (Figma example `10%`). |
| **Track + two thumbs** | Single rail; **two** handles (start/end). Figma places thumbs with absolute positioning; implementation should compute positions from `min`, `max`, `low`, `high`. |
| **Trailing value field** | Optional; shows **high** end (Figma example `50%`). |
| **Min/max labels** | At track ends inside the flexible track region (`gap: var(--space-sm)`). |

**Row gap** between outer fields and track: `var(--space-xl)`.

---

## 3. Variants

| Variant | When to use |
|---------|-------------|
| **Single** | One value (one thumb). |
| **Range** | Min/max pair (two thumbs); enforce `low <= high`. |
| **Without value field** | Space-constrained UI; value available via `aria-valuetext`, tooltip, or label only. |
| **With value field** | Precise readout or optional direct edit. |

---

## 4. Dimensions (default)

| Element | Size | Notes |
|---------|------|--------|
| Track height | `0.25rem` (4px) | `var(--space-xs)`. |
| Thumb hit target | `3rem × 3rem` (48px) | Keep for pointer/touch; center visual thumb. |
| Thumb visual (rest) | ~`1.125rem` (18px) | Circle; tokenize as closest size scale if needed. |
| Value field | `min-width: 4rem` (64px), `min-height: 2.5rem` (40px) | Matches **input** medium height. |
| Value badge (optional, hover) | ~`24px` tall pill, `text-xs` semibold | Figma `.parts/counter`: `background: var(--color-primary)`, `color: var(--color-primary-on)`, `border-radius: var(--radius-sm)`, small shadow. Optional live value above thumb while dragging. |

**Layout stability:** Do not change **track thickness** on focus; use **outline** / focus rings on the thumb hit area (Figma **Focused** adds concentric rings 32px / 16px / 20px inside the 48px box — implement with `outline` + token colors per global policy, or SVG ring that does not reflow the rail).

---

## 5. Interaction states

| State | Track / fill | Thumb | Labels & value field |
|-------|----------------|-------|----------------------|
| **Default** | Filled segment: `semantic.primary.default`; unfilled: neutral (e.g. `semantic.secondary.muted` or primitive slate/gray band — match contrast ≥3:1 for UI components). | Primary or neutral fill per Figma asset mapping. | Text `semantic.text.primary`. |
| **Hover (thumb)** | Unchanged. | Larger ring affordance (Figma: 32px + 20px layers); optional **value badge** with current value. | — |
| **Pressed** | — | Optional pressed affordance (drill Figma `State=Pressed` 24057:591 / 37386:859). | — |
| **Focus visible** | — | Concentric focus treatment inside 48px target (Figma **Focused**); must meet **≥2px** visible focus indicator (accessibility policy). | Value field gets **input**-style `:focus-visible` when focused. |
| **Disabled** | Muted track assets (lighter fill/unfill). | Muted thumb graphic. | `color: var(--color-text-muted)` on labels; value field: `background: var(--color-bg-subtle)` or `semantic.background.subtle`, `border-color: var(--color-border-disabled)`, text muted (Figma `reference/surface-disabled`, `border/disabled`, `text/disabled`). `pointer-events: none` on slider. |

**Disabled label:** `color: var(--color-text-muted)` (Figma `text/disabled`).

---

## 6. Design tokens reference (Figma → semantic)

| Role | Semantic token | Typical CSS variable | Figma variable (reference only) |
|------|----------------|----------------------|-----------------------------------|
| Label / min-max text | semantic.text.primary | `--color-text-primary` | text/primary |
| Muted (disabled) | semantic.text.muted | `--color-text-muted` | text/disabled |
| Filled range | semantic.primary.default | `--color-primary` | reference/primary, surface/button-primary |
| On-primary (badge text) | semantic.primary.onPrimary | `--color-primary-on` | text/onprimary |
| Value field background | semantic.background.subtle | `--color-bg-subtle` | surface/input |
| Value field border (rest) | (input border) | `--color-border-input` | border/input, border/strong |
| Disabled field surface | semantic.background.subtle | `--color-bg-subtle` | reference/surface-disabled |
| Disabled field border | (primitive gray.400) | `--color-border-disabled` | border/disabled |
| Unfilled track | semantic.secondary.muted | `--color-secondary-muted` | (SVG / composite — align to neutral rail) |

**Gap — track fills:** Figma uses raster/SVG track segments; implementations should rebuild fills with CSS (`linear-gradient`, two `div`s, or `progress`-like split) using **semantic** colors above, not embedded hex from exports.

**Gap — value field border width:** Figma shows **`border: 2px`** on the auxiliary field. **input.md §6.1** requires **1px** borders on inputs to avoid layout shift on focus. Prefer **`1px`** + `outline` for focus on the value field to match **input**; if product must match Figma 2px, document exception and keep width constant across states.

**Gap — focus ring asset:** Figma uses layered vectors inside the handle; map to `outline: var(--focus-ring-width) solid var(--focus-ring-color)` and `outline-offset` where possible.

---

## 7. Accessibility

- **Roles**: Expose **`role="slider"`** on the focusable thumb (or native `input type="range"` with styled track/thumb if semantics are preserved). For **range**, two thumbs: each thumb is a slider with appropriate `aria-valuemin`, `aria-valuemax`, `aria-valuenow`; associate with **`aria-labelledby`** / **`aria-label`** for each end, or a single group label with instructions.
- **Values**: Set `aria-valuemin`, `aria-valuemax`, `aria-valuenow`; use **`aria-valuetext`** for human strings (e.g. “50 percent”, “March 2024”).
- **Keyboard**: **Arrow keys** adjust by `step`; **Home/End** to min/max; **Page Up/Page Down** for larger steps (optional, product-defined). Range: tab between thumbs.
- **Live updates**: While dragging, optional **`aria-live="polite"`** on a visually hidden region for screen reader feedback (avoid announcing every pixel — throttle).
- **Contrast**: Track segments and thumb meet **WCAG 2.2** for non-text contrast (3:1) where they are meaningful UI; text on field meets 4.5:1.

Reference: [ARIA slider pattern](https://www.w3.org/WAI/ARIA/apg/patterns/slider/), `.cursor/rules/accessibility.mdc`.

---

## 8. Motion

Short transitions on thumb position optional (`policies/motion.md`). Respect **`prefers-reduced-motion`**: disable elastic/thumb scale animations if any.

---

## 9. Implementation hooks (example)

### CSS classes (BEM-style)

| Class | Purpose |
|-------|---------|
| `ds-slider` | Vertical stack (label + row). |
| `ds-slider__label` | Label element. |
| `ds-slider__row` | Horizontal: track group ± value fields. |
| `ds-slider__track` | Rail container (relative positioning). |
| `ds-slider__track-fill` | Filled portion. |
| `ds-slider__track-rest` | Unfilled portion. |
| `ds-slider__thumb` | 48px hit target + visual thumb. |
| `ds-slider__value` | Value field wrapper (optional). |
| `ds-slider--disabled` | Disabled state on root. |
| `ds-slider--range` | Range variant (two thumbs). |

### Props (React-style reference)

| Prop | Type | Default |
|------|------|---------|
| `min` | `number` | `0` |
| `max` | `number` | `100` |
| `step` | `number` | `1` |
| `value` | `number` | — |
| `defaultValue` | `number` | — |
| `onValueChange` | `(n: number) => void` | — |
| `showValueField` | `boolean` | `true` (Figma default) |
| `disabled` | `boolean` | `false` |
| `mode` | `'single' \| 'range'` | `'single'` |
| `low` / `high` | `number` | — (range) |

---

## 10. Do's and don'ts

- **Do** keep the **48px** thumb target for pointer/touch; center the visible thumb inside it.
- **Do** wire **keyboard** and **ARIA** for every draggable thumb.
- **Do** use **semantic tokens** for fill/rest tracks and disabled states.
- **Don't** rely on Figma export hex in code — rebuild with tokens.
- **Don't** steal focus from the active thumb on every `mousemove` during drag.

---

## 11. Example usage (AI-readable)

**Volume (single):** Label “Volume”, min “0”, max “100”, `value` 50, `showValueField` with “50%”, `aria-valuetext="50 percent"`. Thumb focusable; arrows change by 2.

**Price range:** Label “Price range”, mode `range`, leading field `10%`, trailing `50%` or currency strings via `aria-valuetext`, two thumbs constrained so low ≤ high.

---

*Spec version: 1.0 — Derived from Figma Desktop MCP on nodes listed in Meta; token-mapped for codegen.*
