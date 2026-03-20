# Design tokens

All token files live in this folder (`design-system/tokens/`). They are **AI-readable**: structured for consumption by code generators, design tools, and assistants.

**CSS mirrors:** Default and Lenovo theme variables live in [`design-system/css/`](../css/) (`design-tokens.css`, `design-tokens-lenovo.css`). Import those in your app; you do not need Storybook.

## typography.json

**Contents**: Font family, font size, font weight, line-height, and letter-spacing scales.  
**Use**: Primary reference for type in UI and Tailwind (e.g. `fontFamily.sans`, `fontSize.base`, `fontWeight.semibold`). See `design-system/policies/typography.md` for usage.

## color-primitives.json

**Source**: Extracted from Figma via MCP `get_variable_defs`, **plus** namespaced **`primitive.lenovo.*`** palettes (Lenovo Zero-Floor — not from the default Figma extract).  
**Contents**: Core color primitives — palette scales (50–900) and `common.white` / `common.black`.  
**Excluded**: Semantic tokens (e.g. `text/primary`, `surface/*` roles, `border/weak`, `brand/signatureRed`) live in Figma and reference these primitives.

**Structure**:
- `primitive.common`: white, black
- `primitive.<palette>`: slate, gray, zinc, neutral, stone, red, orange, amber, yellow, lime, green, emerald, teal, cyan, sky, blue, indigo, violet, purple, fuchsia, pink, rose
- `primitive.lenovo`: `primary`, `secondary`, `neutral`, `accent` (brand extension)
- Each token: `{ "value": "#HEX", "type": "color" }` (DTCG-style)

**Use**: Theming, semantic token derivation, contrast checks (WCAG). Re-sync from Figma when the design file’s variables change (Lenovo palettes updated only when brand guidelines change).

## color-semantic.json

**Source**: Derived from `color-primitives.json`; references primitives only.  
**Contents**: Semantic roles — primary, secondary, background, text, success, warning, danger — with default, hover, muted, and on-* variants where applicable.  
**Structure**: `semantic.<role>.<variant>`; each token has `value` (primitive reference), `type`, and `description` (usage + contrast).

**Use**: UI code and AI codegen should prefer semantic tokens over raw primitives so theming and WCAG contrast stay consistent. See `color-semantic.md` for usage guidance and contrast notes (4.5:1 text, 3:1 UI). For **background.base → card → modal stacking** and how `semantic.background.*` layers work, see `surfaces.md`.

## color-semantic-dark.json

**Source**: Derived from `color-primitives.json`; same `semantic.<role>.<variant>` keys as `color-semantic.json`, dark-appropriate primitive references.  
**Contents**: Dark theme semantic roles — primary, secondary, background, text, success, warning, danger — primitives-only, same structure as light.

**Use**: Map the **same** CSS variable names as light; load values from this file under dark theme (`[data-theme="dark"]`, `.dark`, or `prefers-color-scheme: dark` per product). See `color-semantic-dark.md` for tables and theme-switching guidance.

## color-semantic-dark.md

**Contents**: AI-readable dark theme reference — token tables, contrast notes on dark surfaces, implementation note (one set of CSS vars, two token sources).  
**When to read**: Dark mode UI, theme-aware layouts, auditing contrast on dark backgrounds, or `prefers-color-scheme: dark`.

## color-semantic-lenovo.json

**Source**: Derived from `color-primitives.json` (`primitive.lenovo.*` and `common.white` only).  
**Contents**: Lenovo Zero-Floor **light** semantics — same core roles as `color-semantic.json` plus `border`, `link`, `interactive`, `feedback`, and extra `background` / `text` roles. **Success** uses on-brand blue (not green).  
**When to read**: Any **Lenovo-branded** UI with `data-theme="lenovo"` (or equivalent).

## color-semantic-dark-lenovo.json

**Contents**: **Provisional** Lenovo dark semantics; same keys as `color-semantic-lenovo.json`. **Contrast audit required** before production.  
**When to read**: Dark Lenovo surfaces; treat as draft until validated.

## lenovo-zero-floor.md

**Contents**: Brand rules (purple primary, restricted red), theme switching, CSS variable map, conflicts with default specs (e.g. button weight), pointers to policies.  
**When to read**: First Lenovo theme task of a session; onboarding to Lenovo Zero-Floor in this repo.

## typography-lenovo.json

**Contents**: Lenovo font stacks (`LenovoSans`, `Inter`, …), allowed weights (400 / 500 / 700 only), role notes. Does **not** replace `typography.json` globally.  
**When to read**: Lenovo-themed typography or when avoiding weight 600 on Lenovo surfaces.

## spacing.json

**Contents**: Spacing scale `spacing.xs` through `spacing.3xl`, plus `xsPlus` (6px), `lgPlus` (20px), `2xlWide` (40px) for Lenovo Zero-Floor rhythm — dimension tokens in **rem** (4px-based ladder; includes 12px `md` for inputs and buttons).  
**Use**: Padding, margin, gap, and layout spacing. Map to CSS variables (e.g. `var(--space-md)`). See `spacing.md` for the token table, CSS property rules, and usage guidance.

## spacing.md

**Contents**: AI-readable reference — token → rem → typical `--space-*` variable, which properties may use spacing tokens, when to use each step, and codegen rules.  
**When to read**: Any padding, margin, gap, stack spacing, or layout rhythm not fully specified in a component or page doc.
