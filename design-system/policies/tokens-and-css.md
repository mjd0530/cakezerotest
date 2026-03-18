# Tokens and CSS properties — implementation

When turning component specs into code, every spec property must map to the **correct CSS property**. Using a token with a utility or class that sets a different property is a bug.

---

## 1. Principle

Every spec property (background, color, font-size, line-height, font-family, padding, border, etc.) must be implemented by code that sets that **exact** CSS property.

- No hardcoded values.
- No token applied via a utility that targets a different property.

---

## 2. Typography

Font size, line-height, and font-family are **separate** CSS properties. Use CSS (or utilities) that set `font-size`, `line-height`, and `font-family` — not a utility that only sets color.

### Tailwind pitfall

In Tailwind, `text-*` utilities set **color**, not font-size. Do not use `text-[var(--font-size-sm)]` for label size; that sets color.

**Correct approaches:**

- Custom classes that set `font-size: var(--font-size-sm)` and `line-height: var(--line-height-tight)`.
- Theme-extended font/line-height utilities (e.g. extend `theme.fontSize`, `theme.lineHeight` with token values from `design-system/tokens/typography.json`).
- Inline style with the token.

**References:** `design-system/tokens/typography.json`, `design-system/policies/typography.md`.

---

## 3. Colors

- Use **semantic tokens only** (e.g. `--color-primary`, `--color-secondary-muted-hover`).
- Never hardcode hex or rgba (e.g. `#E2E8F0`).
- If a state (e.g. secondary hover) has no token, add one to the theme and reference it.

**References:** `design-system/tokens/color-semantic.json`, `design-system/tokens/color-semantic.md`.

---

## 4. Before finishing a component

For each visual property in the spec (background, color, font-size, line-height, padding, border, etc.), confirm the code sets that **exact** property.

- No hardcoded values.
- No token applied via a utility that targets a different property.

---

## 5. Related docs

- `design-system/tokens/README.md` — Token overview and usage.
- `design-system/policies/typography.md` — Typography roles and no hardcoded type values.
- `design-system/tokens/color-semantic.md` — Semantic color usage and contrast.
