# Design tokens

All token files live in this folder (`design-system/tokens/`). They are **AI-readable**: structured for consumption by code generators, design tools, and assistants.

## typography.json

**Contents**: Font family, font size, font weight, line-height, and letter-spacing scales.  
**Use**: Primary reference for type in UI and Tailwind (e.g. `fontFamily.sans`, `fontSize.base`, `fontWeight.semibold`). See `design-system/policies/typography.md` for usage.

## color-primitives.json

**Source**: Extracted from Figma via MCP `get_variable_defs`.  
**Contents**: Core color primitives only — palette scales (50–900) and `common.white` / `common.black`.  
**Excluded**: Semantic tokens (e.g. `text/primary`, `surface/canvas`, `border/weak`, `brand/signatureRed`) live in Figma and reference these primitives.

**Structure**:
- `primitive.common`: white, black
- `primitive.<palette>`: slate, gray, zinc, neutral, stone, red, orange, amber, yellow, lime, green, emerald, teal, cyan, sky, blue, indigo, violet, purple, fuchsia, pink, rose
- Each token: `{ "value": "#HEX", "type": "color" }` (DTCG-style)

**Use**: Theming, semantic token derivation, contrast checks (WCAG). Re-sync from Figma when the design file’s variables change.

## color-semantic.json

**Source**: Derived from `color-primitives.json`; references primitives only.  
**Contents**: Semantic roles — primary, secondary, background, text, success, warning, danger — with default, hover, muted, and on-* variants where applicable.  
**Structure**: `semantic.<role>.<variant>`; each token has `value` (primitive reference), `type`, and `description` (usage + contrast).

**Use**: UI code and AI codegen should prefer semantic tokens over raw primitives so theming and WCAG contrast stay consistent. See `color-semantic.md` for usage guidance and contrast notes (4.5:1 text, 3:1 UI).

## color-semantic-dark.json

**Source**: Derived from `color-primitives.json`; same `semantic.<role>.<variant>` keys as `color-semantic.json`, dark-appropriate primitive references.  
**Contents**: Dark theme semantic roles — primary, secondary, background, text, success, warning, danger — primitives-only, same structure as light.

**Use**: Map the **same** CSS variable names as light; load values from this file under dark theme (`[data-theme="dark"]`, `.dark`, or `prefers-color-scheme: dark` per product). See `color-semantic-dark.md` for tables and theme-switching guidance.

## color-semantic-dark.md

**Contents**: AI-readable dark theme reference — token tables, contrast notes on dark surfaces, implementation note (one set of CSS vars, two token sources).  
**When to read**: Dark mode UI, theme-aware layouts, auditing contrast on dark backgrounds, or `prefers-color-scheme: dark`.

## spacing.json

**Contents**: Spacing scale `spacing.xs` through `spacing.3xl` — dimension tokens in **rem** (4px-based ladder; includes 12px `md` for inputs and buttons).  
**Use**: Padding, margin, gap, and layout spacing. Map to CSS variables (e.g. `var(--space-md)`). See `spacing.md` for the token table, CSS property rules, and usage guidance.

## spacing.md

**Contents**: AI-readable reference — token → rem → typical `--space-*` variable, which properties may use spacing tokens, when to use each step, and codegen rules.  
**When to read**: Any padding, margin, gap, stack spacing, or layout rhythm not fully specified in a component or page doc.
