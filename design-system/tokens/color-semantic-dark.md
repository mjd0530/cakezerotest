# Semantic color tokens (dark theme) — AI-readable reference

**Light theme:** See [`color-semantic.md`](color-semantic.md) and [`color-semantic.json`](color-semantic.json).

Dark theme semantic tokens use the **same role paths** as light (`semantic.primary.default`, `semantic.background.base`, etc.) but reference **primitives** tuned for dark UI. All values resolve through [`color-primitives.json`](color-primitives.json) only; no hex in this document’s token definitions.

---

## Theme mapping for implementations

Use the **same CSS custom property names** as light (e.g. `--color-text-primary`, `--color-bg-background`, `--color-primary` per component specs). **Scope values** by theme:

- **Light:** map variables from `color-semantic.json`.
- **Dark:** map variables from `color-semantic-dark.json`.

Example pattern: `:root { … }` for light, `[data-theme="dark"]` or `.dark { … }` for dark (product choice). Agents should **not** invent parallel variable names such as `--color-text-primary-dark`.

---

## 1. Primary

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.primary.default` | `primitive.blue.400` | Primary buttons, links, focus rings on dark page background. | Pair with `semantic.primary.onPrimary` (white) on fills; ≥4.5:1. |
| `semantic.primary.hover` | `primitive.blue.300` | Hover for primary controls. | UI emphasis; lighter than default on dark UI. |
| `semantic.primary.muted` | `primitive.blue.900` | Primary tint backgrounds. | Text: `primitive.blue.100` or `blue.200` for ≥4.5:1. |
| `semantic.primary.onPrimary` | `primitive.common.white` | Text/icons on primary fill. | White on blue.400 ≥4.5:1. |

**Usage guidance:** Same as light: one primary action per context; use `danger` for destructive actions.

---

## 2. Secondary

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.secondary.default` | `primitive.slate.500` | Secondary buttons, links, borders. | With `onSecondary` (white) on filled buttons; ≥4.5:1. |
| `semantic.secondary.hover` | `primitive.slate.400` | Hover for secondary controls. | Visible lift on dark backgrounds. |
| `semantic.secondary.muted` | `primitive.slate.800` | Neutral secondary surfaces. | Text: `slate.100` or `slate.200` for ≥4.5:1. |
| `semantic.secondary.onSecondary` | `primitive.common.white` | Text/icons on secondary fill. | White on slate.500 ≥4.5:1. |

---

## 3. Background

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.background.base` | `primitive.gray.900` | Page and app root **background** (dark). | Pair with `text.primary` (gray.50), `text.secondary` (gray.400). |
| `semantic.background.surface` | `primitive.gray.800` | Cards, panels. | Stepped above `background.base`; `text.primary` on surface ≥4.5:1. |
| `semantic.background.surfaceElevated` | `primitive.gray.700` | Modals, popovers, dropdowns. | Border: `gray.600` or `zinc.600` for ≥3:1 UI contrast. |
| `semantic.background.subtle` | `primitive.zinc.800` | Disabled areas, subtle sections. | Text: `gray.300` or `zinc.300` for ≥4.5:1. |

**Usage guidance:** Stack **background.base** → surface → surfaceElevated for depth, same mental model as light.

---

## 4. Text

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.text.primary` | `primitive.gray.50` | Body, headings on dark backgrounds. | Gray.50 on gray.900 ≥4.5:1. |
| `semantic.text.secondary` | `primitive.gray.400` | Supporting text, labels. | On `background.base` / surface ≥4.5:1. |
| `semantic.text.muted` | `primitive.gray.500` | Placeholders, hints, disabled. | Prefer large/non-critical UI; verify on each background. |
| `semantic.text.inverse` | `primitive.gray.900` | Text on *light* accents in dark mode. | For light chip/surface; ensure background is light enough. |

**Accessibility:** Normal text **≥4.5:1**; large text **≥3:1**; UI graphics **≥3:1** vs adjacent colors.

---

## 5. Success

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.success.default` | `primitive.green.400` | Success text, icons on dark UI. | On gray.900 ≥4.5:1. |
| `semantic.success.muted` | `primitive.green.900` | Success backgrounds. | Text: `green.200` or `green.300` for ≥4.5:1. |
| `semantic.success.onSuccess` | `primitive.common.white` | Text/icons on success fill. | White on green.600 ≥4.5:1. |

---

## 6. Warning

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.warning.default` | `primitive.amber.400` | Warnings on dark UI. | On gray.900 ≥4.5:1. |
| `semantic.warning.muted` | `primitive.amber.900` | Warning backgrounds. | Text: `amber.200` or `amber.300` for ≥4.5:1. |
| `semantic.warning.onWarning` | `primitive.amber.100` | Text on warning fills. | Tune vs fill step; `common.white` on `amber.500` if product standardizes light-on-amber. |

---

## 7. Danger

| Token | Primitive reference | Usage | Contrast note |
|-------|---------------------|--------|----------------|
| `semantic.danger.default` | `primitive.red.400` | Errors, destructive affordances. | On gray.900 ≥4.5:1. |
| `semantic.danger.hover` | `primitive.red.300` | Hover for danger controls. | — |
| `semantic.danger.muted` | `primitive.red.900` | Error backgrounds. | Text: `red.200` or `red.300` for ≥4.5:1. |
| `semantic.danger.onDanger` | `primitive.common.white` | Text/icons on danger fill. | White on red.500/600 ≥4.5:1. |

**Usage guidance:** Destructive flows still require confirmation per accessibility / intent docs.

---

## Accessibility contrast summary (dark)

- Pair **text.primary** with **background.base** or **surface** as documented.
- **Focus rings:** Keep ≥3:1; use `semantic.primary.default` or equivalent on dark surfaces.
- Prefer **semantic** names in codegen; resolve light vs dark from the correct JSON per active theme.

All tokens in [`color-semantic-dark.json`](color-semantic-dark.json) reference only `color-primitives.json`.
