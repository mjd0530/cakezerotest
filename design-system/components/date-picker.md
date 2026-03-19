# Date picker — complete component spec

<!-- Design system component: date and range selection with calendar popover. AI-readable. Use design-system tokens only; map each spec property to the correct CSS property (see policies/tokens-and-css.md). Reference: accessibility.md, iconography.md, components/input.md, components/button.md, components/modal.md, intents/form-submission.md. -->

---

## Meta

```yaml
document_type: component_spec
component_id: date_picker
scope: forms | filters | scheduling | settings
audience: ai_consumers | developers | designers
version: 1.0
figma_source: >-
  MCP get_design_context / get_variable_defs on Figma nodes:
  frame "RV/calendar-picker" (2203:36043);
  symbol "state=default" calendar panel (2203:36044);
  variants "state=selected 1" (2203:36204), "state=selected 2" (2203:36366);
  "date input" type=single (2203:35903), type=double (2242:38472).
  Month/year picker variants exist in file (2203:36448, 2203:36480); drill with MCP when extending specs.
token_sources:
  - design-system/tokens/color-semantic.json
  - design-system/tokens/typography.json
  - design-system/tokens/spacing.json
  - design-system/tokens/color-primitives.json
spacing_scale: space.xs | space.sm | space.md | space.lg | space.xl | space.2xl | space.3xl (see design-system/tokens/spacing.md)
radius: radius.sm (4px, segmented inputs); radius.md (8px, panel, day cells, header controls — align with components/button.md ghost / card)
```

---

## 1. Purpose

The date picker lets users choose one date or a start/end range using **segmented text fields** (MM / DD / YYYY) and an optional **calendar popover** opened from a calendar icon. Use it when typed dates need validation, locale-aware formatting, or visual calendar assistance.

**When to use**: Booking, reports, filters, profile or settings forms where a specific calendar day matters.

**When not to use**: Free-form text that is not a date; use `components/input.md`. For a simple native control without custom UI, a hidden or visible native `input type="date"` may suffice at product level — this spec describes the **custom** RV pattern from Figma, not the browser default chrome.

---

## 2. Structure

### 2.1 Trigger field (`date input`)

| Part | Required | Description |
|------|----------|-------------|
| **Label** | Yes | e.g. "Select date", "Start date", "End date" — sentence case per `policies/voice-and-tone.md`. Associate with fields via `for`/`id` or wrapping. Typography matches **input** label: `font-size: var(--text-sm)`, `font-weight: 600`, `line-height: var(--line-height-body-m)`, `font-family: var(--font-sans)`, `color: var(--color-text-primary)`. |
| **Segment group** | Yes | Three regions for month, day, year separated by visible `/` characters (not inputs). Each segment uses the same **control surface** rules as **input** medium (height, border, padding, placeholder). |
| **Separators** | Yes | Slash between segments: `font-size: var(--text-lg)`, `font-weight: 600`, `line-height` per `typography.lineHeight.titleM` (or nearest title scale), `color: var(--color-text-secondary)` (Figma `reference/helper`). |
| **Calendar trigger** | Optional (default on in Figma) | **Icon-only** `button type="button"`, 40×40px hit target, `border-radius: var(--radius-md)`, Material **calendar_today** (Outlined). `aria-label` e.g. "Open calendar". |
| **Helper / error** | Optional | Same row patterns as **input** §8–9; link with `aria-describedby`. |

**Layout (single)** — Figma: label stack `gap: var(--space-xs)`; row of segments + trigger `gap: var(--space-lg)` (16px).

**Layout (range / double)** — Two labeled columns side by side; Figma `gap: var(--space-3xl)` (48px) between columns. Each column repeats the single pattern. Shared helper below full width.

Segment internals: use native `<input inputmode="numeric">` or similar with `maxlength` and parsing, or a single combobox pattern — implementation choice; visually each segment matches **input** §4 medium (40px height, `padding` `var(--space-sm)` vertical / `var(--space-md)` horizontal where applicable). Placeholder tokens: `color: var(--color-text-secondary)` for "MM", "DD", "YYYY".

### 2.2 Calendar panel (`RV/calendar-picker`)

| Part | Required | Description |
|------|----------|-------------|
| **Panel container** | Yes | Elevated surface: `background` from `semantic.background.surfaceElevated` (light theme: **white**, same as **surface** — typical CSS `var(--color-bg-surface-elevated)` or `var(--color-bg-surface)`; separate from **base** with border + shadow). `border: 1px solid` neutral border (primitive `gray.200` / weak border per `color-semantic.md` §3). `border-radius: var(--radius-md)`. Shadow: Figma "Elevation Light/2" (dual drop shadow). **Gap:** no shadow token in repo — add `--shadow-elevation-2` in theme or use motion/elevation doc when defined. |
| **Header** | Yes | Height band Figma ~80px: use `padding: var(--space-xl) var(--space-xl)` and vertically center row; `border-bottom: 1px solid` same as panel border. Contains: previous month **icon button** (Material `chevron_left`), **month** and **year** controls (Figma: secondary-style toggle buttons, 32px height, `radius-md`), next month **icon button** (`chevron_right`). |
| **Weekday row** | Yes | Seven columns, each cell `min-width`/`width` aligned to day column (Figma ~50px). Weekday label: `font-size: var(--text-base)`, `font-weight: 600`, `line-height: var(--line-height-body-m)`, `color: var(--color-primary)` (maps Figma `text/onCanvas-primary` to semantic primary accent on light surface). |
| **Day grid** | Yes | 6×7 logical grid; `gap: var(--space-sm)` between cells (Figma 8px). Each day is a **button** `type="button"`. |
| **Footer** | Yes | Figma: `border-top`, same horizontal padding as header; **Today** (secondary button style left), **Cancel** (ghost/text) + **OK** (primary) right group; `gap: var(--space-lg)` between primary actions. |

**Figma variant set** (for visual QA): `default`, `hover 1`, `selected 1`, `hover 2`, `selected 2`, `month picker`, `year picker`. Implementations should match default + selected states at minimum; hover per §6.

---

## 3. Variants

| Variant | When to use | Notes |
|---------|-------------|--------|
| **Single date** | One value. | One trigger field + one popover instance. |
| **Range** | Start and end. | Two trigger columns; one or two popovers per product (Figma shows two fields). `selected 2` shows **two** primary-filled days (endpoints); middle dates use default styling in current Figma — optional **in-range** styling: **Gap:** use `semantic.primary.muted` fill between endpoints if product requires clearer range affordance (not shown between 14–19 in Figma). |
| **Month / year drill-down** | Fast navigation. | Separate views (`month picker`, `year picker` symbols); reuse panel chrome and secondary button tiles. |

---

## 4. Sizes (unified)

| Element | Dimension / rule |
|---------|------------------|
| Segment control height | `2.5rem` (40px) — same as **input** medium. |
| Calendar trigger button | `2.5rem × 2.5rem` (40px), icon 24px (Material default). |
| Day cell | Figma ~50px × 40px min hit target; keep **minimum 44×44px** touch target if product requires mobile (WCAG 2.5.5 target size — AAA; still recommended). |
| Month/year header controls | 32px height (`2rem`), `text-sm` semibold (Figma 14/20). |
| Header/footer vertical band | Figma 80px: achieve with `padding: var(--space-xl)` top/bottom + centered 32px or 40px controls (`min-height` via content, or `calc(var(--space-xl) + var(--space-xl) + 2rem)` ≈ 80px at 16px root). |

Typography: **font-family** always `var(--font-sans)` (design tokens — Figma export used Segoe UI; apps use Rookery stack from `typography.json`).

---

## 5. Layout and spacing

| Relationship | Token / rule |
|--------------|----------------|
| Label → field | `gap: var(--space-xs)`. |
| Segments ↔ calendar icon | `gap: var(--space-lg)`. |
| Between segment mini-fields | `gap: var(--space-xs)` inside group (Figma 4px). |
| Calendar panel outer padding | `padding: var(--space-lg)` on grid body (Figma 16px). |
| Panel header/footer padding | `padding-left/right: var(--space-xl)`, vertical per §4. |
| Day grid gap | `gap: var(--space-sm)` row and column. |
| Range: two columns | `gap: var(--space-3xl)` between start and end stacks. |

---

## 6. Interaction states

### 6.1 Layout stability

Same rule as **input** §6.1: **do not** change border width on focus for segment fields; use `outline` / focus ring ≥ 2px for `:focus-visible`.

### 6.2 Day cell states

| State | Visual (token mapping) |
|-------|-------------------------|
| **Default (in-month)** | Text `var(--color-text-primary)`, transparent background, `border-radius: var(--radius-md)`. |
| **Hover** | **Gap in Figma export:** explicit hover fill not required in default symbol; use `background: var(--color-secondary-muted)` or lighter primary tint on `:hover` (align with **button** secondary ghost hover). |
| **Focus visible** | Focus ring on button; do not rely on color alone. |
| **Selected** | `background: var(--color-primary)`, text `var(--color-primary-on)` (semantic `primary.onPrimary`). |
| **Outside month / disabled** | Text `var(--color-text-muted)`; `pointer-events: none` or `disabled` per logic; no primary fill. |
| **Today (optional)** | If distinct from selected, use outline or `border: 1px solid var(--color-primary)` without changing layout width (1px border on all states for that cell). |

### 6.3 Footer actions

Map **Today** to secondary button tokens (**button** variant secondary). **Cancel** ghost/text; **OK** primary — see **button** §6–7.

---

## 7. Design tokens reference (Figma → semantic)

| Role | Semantic token | Typical CSS variable | Figma variable (reference only) |
|------|----------------|----------------------|----------------------------------|
| Panel surface | semantic.background.surfaceElevated | `--color-bg-surface-elevated` or `--color-bg-surface` (light; white) | surface/card |
| Panel border | (weak neutral) | `--color-border-subtle` | border/weak |
| Primary fill (selected day, OK) | semantic.primary.default | `--color-primary` | surface/button-primary |
| On-primary text | semantic.primary.onPrimary | `--color-primary-on` | text/onPrimary |
| Secondary fill (month/year chips, Today) | semantic.secondary.muted | `--color-secondary-muted` | surface/button-secondary |
| On-secondary text | semantic.text.primary | `--color-text-primary` | text/onSecondary |
| Weekday accent | semantic.primary.default | `--color-primary` | text/onCanvas-primary |
| In-month day text | semantic.text.primary | `--color-text-primary` | text/primary |
| Muted / out-of-month | semantic.text.muted | `--color-text-muted` | text/disabled |
| Segment empty / placeholder | semantic.text.secondary | `--color-text-secondary` | reference/helper, border/input |
| Segment background empty | semantic.background.subtle | `--color-bg-subtle` | surface/input |
| Icon default | semantic.text.primary | `--color-text-primary` (icon color) | icon/primary |

**Gap — border alias tokens:** Same as **input.md** §7: dedicated weak border on elevated surfaces may use `--color-border-subtle` mapped from primitive `gray.200` until semantic `border.subtle` exists.

**Gap — elevation tokens:** Figma "Elevation Light/2" is a fixed double shadow. Map to theme shadow tokens when added; until then use documented `box-shadow` values derived from primitives with opacity, or a single `--shadow-elevation-2` alias.

**Gap — in-range highlight:** Not shown in Figma `selected 2` between endpoints; recommend `semantic.primary.muted` for optional range span.

---

## 8. Accessibility

- **Dialog pattern**: When the calendar opens as a modal overlay, use `role="dialog"`, `aria-modal="true"`, `aria-labelledby` on a visible title (e.g. "March 2024" or "Select date"), and **focus trap** + **Escape** per `components/modal.md` and `.cursor/rules/accessibility.mdc`.
- **Trigger**: Calendar button `aria-expanded`, `aria-controls` pointing to the dialog id; segment inputs labeled (visible label + optional `aria-label` on each segment if design hides redundant text).
- **Grid**: Calendar days in `role="grid"` with `role="columnheader"` for weekdays and `role="gridcell"` for days; or use APG [Date picker dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/examples/datepicker-dialog/) semantics. Ensure **arrow key** navigation within the grid and **Page Up/Down** for month change if implemented.
- **Live region**: Optional `aria-live="polite"` for month changes when not using native title focus announcements.
- **Contrast**: Selected day uses primary on white/onPrimary — verify AA for text and focus rings on panel background (`color-semantic.md`).

---

## 9. Keyboard

| Key | Behavior |
|-----|----------|
| **Tab** | Moves through segments, calendar button, then into dialog (if open) per focus trap order. |
| **Enter / Space** | Activate focused day or footer button. |
| **Escape** | Close dialog; return focus to calendar trigger. |
| **Arrows** | Move between days inside grid (implementation must match grid roles). |
| **Page Up / Down** | Previous/next month when specified by implementation. |

---

## 10. Motion

Short transitions on hover/focus (duration/easing per `policies/motion.md`). Respect `prefers-reduced-motion: reduce`.

---

## 11. Implementation hooks (example)

### CSS classes (BEM-style)

| Class | Purpose |
|-------|---------|
| `ds-date-field` | Vertical stack: label + segment row + helper. |
| `ds-date-field__segments` | Flex row: segments + separators + calendar button. |
| `ds-date-field__segment` | Single segment control (wraps native input). |
| `ds-date-picker-dialog` | Panel root (dialog). |
| `ds-date-picker-dialog__header` | Nav row. |
| `ds-date-picker-dialog__grid` | Weekday + day grid wrapper. |
| `ds-date-picker-dialog__day` | Day button. |
| `ds-date-picker-dialog__day--selected` | Selected state. |
| `ds-date-picker-dialog__footer` | Footer actions. |

### Props (React-style reference)

| Prop | Type | Default |
|------|------|---------|
| `mode` | `'single' \| 'range'` | `'single'` |
| `open` | `boolean` | `false` |
| `onOpenChange` | `(open: boolean) => void` | — |
| `value` / `start` / `end` | ISO string or `Date` | — |
| `showCalendarTrigger` | `boolean` | `true` |
| `invalid` | `boolean` | `false` |

---

## 12. Do's and don'ts

- **Do** reuse **input** specs for segment borders, placeholders, and error rows.
- **Do** use **Material** icons only (`calendar_today`, `chevron_left`, `chevron_right`) with `aria-label` on icon-only controls.
- **Do** restore focus to the trigger when the dialog closes.
- **Don't** use Tailwind `text-*` utilities for font **size** — they set **color** (see `policies/tokens-and-css.md`).
- **Don't** open the calendar without a keyboard-accessible trigger.

---

## 13. Example usage (AI-readable)

**Single date filter**: Render `ds-date-field` with label "Select date", three segments with placeholders MM / DD / YYYY, calendar button with `aria-label="Open calendar"`. On open, render `ds-date-picker-dialog` with title id bound to month/year, grid for current month, footer Today / Cancel / OK. On day click, write ISO date to state, update segments, close dialog, announce "Date selected" via polite live region optional.

**Range**: Two fields "Start date" and "End date" with `gap: var(--space-3xl)`; validate `start <= end`; show `selected 2` style with two primary endpoints; add muted range fill if product adopts the gap note in §3.

---

*Spec version: 1.0 — Derived from Figma Desktop MCP (`get_design_context`, `get_variable_defs`) on nodes listed in Meta; token-mapped for codegen.*
