# Spacing tokens — AI-readable reference

Spacing is the single source for **padding**, **margin**, **gap**, and layout rhythm when building UI from this design system. Values live in `spacing.json` (DTCG-style); this document explains how to apply them in code.

---

## 1. Token table

Map JSON paths to typical CSS custom properties. Implementations should expose `spacing.*` as `--space-*` (kebab-case) in theme / CSS.

| Token path | Value (rem) | Approx px (16px root) | Typical CSS variable |
|------------|-------------|------------------------|----------------------|
| `spacing.xs` | 0.25rem | 4 | `var(--space-xs)` |
| `spacing.xsPlus` | 0.375rem | 6 | `var(--space-xs-plus)` |
| `spacing.sm` | 0.5rem | 8 | `var(--space-sm)` |
| `spacing.md` | 0.75rem | 12 | `var(--space-md)` |
| `spacing.lg` | 1rem | 16 | `var(--space-lg)` |
| `spacing.lgPlus` | 1.25rem | 20 | `var(--space-lg-plus)` |
| `spacing.xl` | 1.5rem | 24 | `var(--space-xl)` |
| `spacing.2xl` | 2rem | 32 | `var(--space-2xl)` |
| `spacing.2xlWide` | 2.5rem | 40 | `var(--space-2xl-wide)` |
| `spacing.3xl` | 3rem | 48 | `var(--space-3xl)` |

**Dot notation in specs** may appear as `space.xs` — treat as `spacing.xs`.

---

## 2. CSS properties

Use spacing tokens only with declarations (or utilities) that set the **correct** property:

- `padding`, `padding-*`
- `margin`, `margin-*`
- `gap`, `row-gap`, `column-gap`
- `inset`, `top`, `right`, `bottom`, `left` when used for layout spacing

Do not use a token meant for spacing on a property that does something else. See `design-system/policies/tokens-and-css.md`.

---

## 3. Usage guidance

| Step | Typical use |
|------|-------------|
| **xs** | Label-to-control gap, tight vertical stacks, error row icon alignment. |
| **xsPlus** | 6px label-to-input gap (Lenovo Zero-Floor). |
| **sm** | Gaps inside controls (text ↔ icon), compact horizontal rhythm. |
| **md** | Input horizontal padding (leading edge), medium gaps between related items. |
| **lg** | Default spacing between form fields, comfortable button padding (with component spec). |
| **lgPlus** | Default card internal padding / 20px rhythm (Lenovo Zero-Floor). |
| **xl** | Section padding, larger gaps between groups. |
| **2xl** | Separation between blocks inside a region. |
| **2xlWide** | 40px section padding desktop (Lenovo Zero-Floor). |
| **3xl** | Major section spacing; aligns with dashboard-scale rhythm. |

**Page-level layout** (gutters, grid gaps) may combine these steps; see `design-system/pages/dashboard.md` (`spacing_scale` reference) and the relevant page spec.

---

## 4. Rules for codegen

- **Do not** hardcode `px` or `rem` for spacing when a token step fits the design.
- **Do** extend `spacing.json` with a new step if a recurring value has no token; add a **gap comment** in consuming specs until the token is merged.
- **Rem** values respect user font-size preferences; avoid px in token definitions.

---

## 5. Related docs

- `design-system/tokens/spacing.json` — Canonical values.
- `design-system/tokens/README.md` — Token folder overview.
- `design-system/policies/tokens-and-css.md` — Token → correct CSS property.
- `design-system/components/button.md`, `design-system/components/input.md` — Component spacing examples.
