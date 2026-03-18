# Typography Rules — Zero Floor Design System
**Version 1.0 · Agentic Reference**

> This document is the typography authority for AI-generated UI. It was built from the Figma Type Scale design so agents have a single reference for type styles, roles, and tokens. Use this doc when generating or validating typography; do not hardcode font sizes, weights, or line-heights.

---

## 1. Typography roles and token mapping

Components reference tokens such as `type.scale.headingLg`, `type.weight.semibold`, `type.lineHeight.relaxed` (see `components/basic-modal.md`, `components/confirmation-modal.md`). Code tokens live in `design-system/tokens/typography.json` and are extended in Tailwind (see `tailwind.config.cjs`).

### Role → type style

| Role | Type style | Use |
|------|------------|-----|
| Display / hero | Editorial XL–XS, Display XL–XS | Large editorial or display headlines |
| Section heading | Headline L–XS | Major section titles |
| Card / panel title | Title XL–XS | Titles, modal titles, list headers |
| UI label / button | Label XL–S, Link XL–S | Labels, buttons, links |
| Body copy | Body L–XS | Paragraphs, table cells, descriptions |
| Caption / metadata | figma-label, Title XS | Captions, timestamps, secondary info |

### Font families

- **Editorial:** Instrument Serif (Editorial XL–XS).
- **Display, Headline, Title, Label, Body, Link:** Rookery New (codebase primary font is **Rookery**; treat "Rookery New" as the same family).

Use `design-system/tokens/typography.json` `fontFamily.sans` for the primary stack; add a serif/editorial entry if the project supports Editorial.

### Weights

- **Editorial:** Regular (400).
- **Display, Headline, Label, Link:** Bold (700).
- **Title:** Medium (500).
- **Body:** Regular (400).

Map to `type.weight.regular`, `type.weight.medium`, `type.weight.semibold`, `type.weight.bold` in component specs and `design-system/tokens/typography.json` (`fontWeight.normal`, `fontWeight.bold`).

---

## 2. Rules (canonical)

- **Minimum font size:** 12px (`--text-xs`). Never smaller.
- **Maximum line length:** 65ch for body copy.
- **Weights:** Prefer 400 and 500 (per `policies/style.md`). Use 700 only for Display, Headline, Label, Link where the type style is Bold.
- **No hardcoded type values:** Use design tokens or CSS variables; never raw px/rem for font size, weight, or line-height in component code.

---

## 3. Related docs

- `policies/style.md` — Type scale (modular 1.25), weights, line-height, heading hierarchy.
- `design-system/tokens/typography.json` — Code-side font family, font size, font weight, line-height.
- `components/basic-modal.md`, `components/confirmation-modal.md` — Token references for titles, body, labels.

---

## 4. Type scale reference (from Figma)

Type styles below were captured from the Figma Type Scale design. Use as the single reference when applying typography.

**Editorial (Instrument Serif)**  
- Editorial XL: 56px / 60 line-height / 0 letter-spacing  
- Editorial L: 52 / 56 / -0.6  
- Editorial M: 48 / 52 / -0.6  
- Editorial S: 44 / 46 / -0.6  
- Editorial XS: 40 / 44 / -0.6  

**Display (Rookery New Bold)**  
- Display XL–XS: 52–36px, line-height 60–44, letter-spacing -0.6 to -0.4  

**Headline (Rookery New Bold)**  
- Headline L–XS: 32–20px, line-height 40–26  

**Title (Rookery New Medium)**  
- Title XL–XS: 22–12px, line-height 28–16  

**Label / Link (Rookery New Bold)**  
- Label XL–S: 16–11px — Label L/M/S, Link XL/L/M/S  

**Body (Rookery New Regular)**  
- Body L–XS: 16–11px, line-height 24–15  

**_fig-doc (Martian Mono)**  
- figma-title: 56px / 64 / -4  
- figma-body: 18px / 24 / -2 (Light)  
- figma-label: 11px / 100% / -1  
- figma-subheading: 16px / 100% / -0.4 (Medium)  
- figma-heading-3: 18px / 100% / 0 (Bold)  
