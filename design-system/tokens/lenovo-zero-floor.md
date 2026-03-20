# Lenovo Zero-Floor theme

AI- and developer-readable reference for the **Lenovo-branded** color and typography layer. This theme **coexists** with the default Agentic tokens (`color-semantic.json`); it does not replace them.

---

## When to use

- Products or surfaces that must follow **Lenovo Zero-Floor** brand rules (purple primary, restricted red, on-brand blue success).
- Apply in code with **`[data-theme="lenovo"]`** on a root element (e.g. `html` or `#root`) and load [`design-tokens-lenovo.css`](design-tokens-lenovo.css) after the default [`design-tokens.css`](design-tokens.css) (or an app-local copy generated from the same token source).

**Canonical token files**

| File | Role |
|------|------|
| [`color-primitives.json`](color-primitives.json) → `primitive.lenovo.*` | Brand palettes (not from default Figma extract) |
| [`color-semantic-lenovo.json`](color-semantic-lenovo.json) | Light semantic roles + extensions |
| [`color-semantic-dark-lenovo.json`](color-semantic-dark-lenovo.json) | **Provisional** dark roles — audit before production |
| [`typography-lenovo.json`](typography-lenovo.json) | Font stacks and weight rules |

Also read [`spacing.json`](spacing.json) for `spacing.xsPlus`, `spacing.lgPlus`, `spacing.2xlWide` (Lenovo rhythm).

---

## Theme switching

- **Default theme:** `color-semantic.json` + default CSS variables (`design-tokens.css` on `:root`).
- **Lenovo theme:** `color-semantic-lenovo.json` + variables under **`[data-theme="lenovo"]`**.
- **Dark Lenovo:** Prefer a single attribute strategy agreed by the product, e.g. `[data-theme="lenovo"][data-color-mode="dark"]`, loading values from `color-semantic-dark-lenovo.json`. Dark Lenovo tokens are **provisional**; verify contrast (WCAG 2.2 AA) before release.

Do not hardcode hex in components — use CSS custom properties that mirror semantic tokens.

---

## Brand rules (summary)

### Purple is primary

- CTAs, active nav, focus rings, and info accents use **Lenovo purple** (`semantic.primary.*` / `--color-interactive-primary` / `--color-primary`).
- **Never** use the red accent scale for primary or generic interactive UI.

### Red is restricted

Use **`primitive.lenovo.accent.*` / feedback.error / semantic.danger`** only for:

- Destructive **buttons** (with confirmation per destructive-action intent)
- **Error** alerts/banners, error badges/chips
- **Invalid** inputs (`aria-invalid`), field error text
- **Lenovo logo** (signature red — `accent.500`)

Do **not** use `red-*`, `rose-*`, or `pink-*` Tailwind classes for interactive elements. Do not use accent red for links, nav, or focus rings.

### Success is on-brand blue

Lenovo maps **success** to **secondary blue** (`semantic.success.*` → `lenovo.secondary`), not green. This **differs** from the default theme (`color-semantic.json` uses green for success).

### Typography

- Stacks: see `typography-lenovo.json` (`LenovoSans`, `Inter`, …).
- **Weights:** 400, 500, 700 only — **no 600** (`font-semibold`) on Lenovo-themed UI.
- **Conflict:** [`design-system/components/button.md`](../components/button.md) defaults to weight **600** for labels; for Lenovo, use **500** (medium) — see § Lenovo theme in that spec.

### Icons

Follow [`design-system/policies/iconography.md`](../policies/iconography.md): **Material Design icons only** (this repo policy). Lenovo Zero-Floor does not change that.

### Accessibility

Follow [`design-system/policies/accessibility.md`](../policies/accessibility.md) and `.cursor/rules/accessibility.mdc` (focus rings, live regions, modals, etc.). Lenovo theme does not relax WCAG.

---

## CSS variable map (light)

Variables are defined in `design-system/tokens/design-tokens-lenovo.css`. Semantic source: `color-semantic-lenovo.json`.

| Semantic path (JSON) | Typical CSS variable |
|------------------------|----------------------|
| `semantic.background.base` | `--color-bg-default`, `--color-bg-background` (alias) |
| `semantic.background.subtle` | `--color-bg-subtle` |
| `semantic.background.*` (muted, emphasis, …) | `--color-bg-muted`, `--color-bg-emphasis`, `--color-bg-inverse`, `--color-bg-brand`, `--color-bg-brand-strong` |
| `semantic.text.*` | `--color-text-default`, `--color-text-subtle`, `--color-text-placeholder`, `--color-text-disabled`, `--color-text-inverse`, `--color-text-brand`, `--color-text-link`, `--color-text-link-hover` |
| `semantic.primary.*` | `--color-primary`, `--color-primary-hover`, `--color-primary-muted`, `--color-primary-on` |
| `semantic.secondary.*` | `--color-secondary`, `--color-secondary-muted`, … |
| `semantic.border.*` | `--color-border-subtle`, `--color-border-default`, `--color-border-strong`, `--color-border-focus`, `--color-border-brand` |
| `semantic.interactive.*` | `--color-interactive-*` |
| `semantic.feedback.*` | `--color-feedback-*` |
| `primitive.lenovo.primary.*` | `--color-lenovo-primary-*` (scale for focus ring / rare direct use — prefer semantics in components) |
| `primitive.lenovo.accent.*` | `--color-accent-*` (feedback/logo only) |

Legacy aliases (`--color-text-primary`, `--color-danger`, …) are set where needed so existing reference components keep working under `data-theme="lenovo"`.

---

## Radius, shadow, motion (reference)

No separate JSON file yet; use tokens as CSS variables under `[data-theme="lenovo"]` in [`design-tokens-lenovo.css`](design-tokens-lenovo.css):

- **Radius:** `--radius-xs` through `--radius-full` (6px default interactive = `--radius-md` 0.375rem).
- **Focus:** `--shadow-focus` uses purple 200 tint.
- **Motion:** `--motion-duration-*`, `--motion-ease-*` aligned with `design-system/policies/motion.md` (150ms default transitions).

---

## Provisional dark theme

[`color-semantic-dark-lenovo.json`](color-semantic-dark-lenovo.json) is a **starting point** only. Re-audit every role against real surfaces and WCAG 2.2 AA before shipping dark Lenovo UI.

---

## Related

- Scoped agent rule: [`.cursor/rules/lenovo-zero-floor.mdc`](../../.cursor/rules/lenovo-zero-floor.mdc)
- Default semantics (for comparison): [`color-semantic.json`](color-semantic.json)
