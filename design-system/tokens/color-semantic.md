# Semantic color tokens — AI-readable reference

**Light theme (this document + `color-semantic.json`).** For **dark theme**, use the same semantic **role names** with [`color-semantic-dark.json`](color-semantic-dark.json) and [`color-semantic-dark.md`](color-semantic-dark.md) — one set of CSS variables, values switched by theme.

Semantic tokens map **primitive** colors (see `color-primitives.json`) to **usage roles**. Use these tokens in UI code and design so theming and accessibility stay consistent. All semantic tokens **reference only** primitives; no hex values are defined here.

---

## 1. Primary

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.primary.default` | `primitive.blue.600` | Primary buttons, main links, selected states, focus rings. One primary action per block. | Use with `semantic.primary.onPrimary` (white) for buttons; exceeds 4.5:1. |
| `semantic.primary.hover` | `primitive.blue.700` | Hover for primary buttons and primary-styled links. | UI element; 3:1 minimum. |
| `semantic.primary.muted` | `primitive.blue.100` | Light primary tint: badges, highlights, subtle backgrounds. | Use text color `primitive.blue.800` or `blue.900` on this background for ≥4.5:1. |
| `semantic.primary.onPrimary` | `primitive.common.white` | Text and icons on primary fill (e.g. button label). | White on blue.600 > 4.5:1. |

**Usage guidance:** Prefer `primary.default` for the single most important action in a view. Do not use primary for destructive actions; use `danger` instead.

---

## 2. Secondary

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.secondary.default` | `primitive.slate.600` | Secondary buttons, secondary links, lower emphasis than primary. | Use with `semantic.secondary.onSecondary` (white) for filled buttons; ≥4.5:1. |
| `semantic.secondary.hover` | `primitive.slate.700` | Hover for secondary controls. | 3:1 for UI. |
| `semantic.secondary.muted` | `primitive.slate.100` | Neutral tint for secondary-style surfaces. | Text on this: use `primitive.slate.700` or darker for ≥4.5:1. |
| `semantic.secondary.onSecondary` | `primitive.common.white` | Text/icons on secondary fill. | White on slate.600 > 4.5:1. |

**Usage guidance:** Use for alternative actions (e.g. Cancel next to Submit). Keeps hierarchy clear when paired with primary.

---

## 3. Background

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.background.base` | `primitive.common.white` | Page and app root **background** (full-bleed shell). | Pair with `text.primary` and `text.secondary`; both meet 4.5:1 on white. |
| `semantic.background.surface` | `primitive.gray.50` | Cards, panels, raised areas. | Slight contrast with `background.base`. Use `text.primary` on surface for ≥4.5:1. |
| `semantic.background.surfaceElevated` | `primitive.common.white` | Modals, dropdowns, popovers. | Use border from `primitive.gray.200` for 3:1 UI element contrast. |
| `semantic.background.subtle` | `primitive.gray.100` | Disabled backgrounds, subtle sections. | Use `primitive.gray.700` or darker for text on subtle for ≥4.5:1. |

**Usage guidance:** Prefer **background.base** → surface → surfaceElevated for stacking (page **background**, then cards, then overlays). Avoid putting body text on `subtle` unless it is large or secondary.

---

## 4. Text

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.text.primary` | `primitive.gray.900` | Body copy, headings. | Gray.900 on white ≥7:1 (WCAG AA). Use for main content. |
| `semantic.text.secondary` | `primitive.gray.600` | Supporting text, captions, labels. | Gray.600 on white ≥4.5:1. |
| `semantic.text.muted` | `primitive.gray.500` | Placeholders, hints, disabled text. | ~4.5:1 on white; prefer for large or non-critical text. |
| `semantic.text.inverse` | `primitive.common.white` | Text on dark backgrounds (primary button, dark header). | Ensure background is dark enough for ≥4.5:1 (e.g. blue.600, slate.700). |

**Accessibility:** Normal text requires **≥4.5:1** contrast; large text (18pt+ or 14pt+ bold) **≥3:1**. All text tokens above are chosen to meet 4.5:1 on their intended backgrounds.

---

## 5. Success

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.success.default` | `primitive.green.600` | Success messages, confirmations, positive indicators. | Green.600 on white ≥4.5:1. |
| `semantic.success.muted` | `primitive.green.100` | Success backgrounds, success badges. | Use `primitive.green.800` or `green.900` for text on muted for ≥4.5:1. |
| `semantic.success.onSuccess` | `primitive.common.white` | Text/icons on success fill (e.g. success button). | White on green.600 > 4.5:1. |

**Usage guidance:** Use for completion states and non-critical positive feedback. Do not use for primary CTAs; use `primary` instead.

---

## 6. Warning

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.warning.default` | `primitive.amber.600` | Caution messages, non-destructive alerts. | Amber.600 on white ≥4.5:1 for normal text. |
| `semantic.warning.muted` | `primitive.amber.100` | Warning backgrounds, warning badges. | Use `primitive.amber.800` or `amber.900` for text for ≥4.5:1. |
| `semantic.warning.onWarning` | `primitive.amber.900` | Text/icons on warning fill (e.g. warning button). | Amber.900 on amber.600 meets 4.5:1. |

**Usage guidance:** Reserve for caution and reversible consequences. For destructive or irreversible actions use `danger` and a confirmation step.

---

## 7. Danger

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.danger.default` | `primitive.red.600` | Destructive actions, errors, critical alerts. | Red.600 on white ≥4.5:1. |
| `semantic.danger.hover` | `primitive.red.700` | Hover for danger buttons and destructive links. | 3:1 for UI. |
| `semantic.danger.muted` | `primitive.red.100` | Error backgrounds, error badges. | Use `primitive.red.800` or `red.900` for text for ≥4.5:1. |
| `semantic.danger.onDanger` | `primitive.common.white` | Text/icons on danger fill (e.g. delete button). | White on red.600 > 4.5:1. |

**Usage guidance:** Use for delete/remove and error states. Per accessibility policy, destructive actions must use a confirmation modal before executing.

---

## Accessibility contrast summary

- **Text (normal):** Use tokens that guarantee **≥4.5:1** on their background (e.g. `text.primary` on `background.base`, `primary.onPrimary` on primary fill).
- **Text (large):** **≥3:1** is minimum; prefer 4.5:1 where possible.
- **UI components and graphics:** **≥3:1** against adjacent backgrounds.
- **Focus rings:** Use primary or a contrasting color at **≥3:1**; minimum 2px visible ring.

All semantic tokens in this file reference only `design-system/tokens/color-primitives.json`. When generating UI, prefer semantic token names over raw primitive names so theming and contrast rules stay consistent.
