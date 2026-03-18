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
