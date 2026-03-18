# Iconography Policy — Zero Floor Design System

**Version 1.0 · Agentic Reference**

> This document is the iconography authority for AI-generated UI. Use it when choosing, implementing, or validating icons. **Only Material Design icons are permitted** in the design system.

---

## 1. Icon source (canonical)

- **Allowed:** [Material Design Icons](https://fonts.google.com/icons) (Material Symbols) and the [Material Design Icons](https://materialdesignicons.com/) icon set.
- **Not allowed:** Custom SVG sets, Font Awesome, Heroicons, Phosphor, Lucide, or any other icon library. Do not introduce icons from other systems.

Use a single, consistent source so visuals and implementation stay aligned across the product.

---

## 2. Naming and usage

- Refer to icons by their **official Material name** (e.g. `add`, `close`, `delete`, `edit`, `settings`, `search`, `arrow_back`, `check_circle`).
- Use **outlined** or **rounded** style by default unless the component spec specifies filled.
- Prefer the **24dp** default size for UI; use 20dp for compact contexts (e.g. inline with label) and 18dp only where space is very tight. Map sizes to design tokens or CSS variables where available.

---

## 3. Implementation

- **Web:** Use [Material Symbols](https://fonts.google.com/icons) (Google Fonts) or the official `@material-icons` / Material Icons package. If using SVG sprites or components, ensure the artwork is from the Material Design Icons set only.
- **Figma / design:** Use only Material Design Icons (or Material Symbols) in libraries and components. Do not paste icons from other kits.
- When generating UI, the agent must select icons from the Material set; if an exact match is missing, choose the closest semantic equivalent from Material Design Icons rather than substituting another library.

---

## 4. Accessibility (icon-specific)

- Icon-only buttons and controls must have an **accessible name** (`aria-label` or visible text). See `.cursor/rules/accessibility.mdc` and `policies/accessibility.md`.
- Decorative icons should be hidden from assistive tech (`aria-hidden="true"`) when they duplicate adjacent text.

---

## 5. Rules (canonical)

- **Only use Material Design icons.** No other icon systems.
- Use official Material names and a consistent style (outlined/rounded default).
- Prefer 24dp; use 20dp or 18dp only when the layout requires it.
- Every interactive icon must have an accessible name; decorative icons must be marked `aria-hidden` when redundant with text.

---

## 6. Related docs

- `.cursor/rules/iconography.mdc` — Cursor rule so the agent applies this policy when generating UI.
- `policies/accessibility.md` and `.cursor/rules/accessibility.mdc` — Accessible names and icon-only button requirements.
- `components/button.md` — Icon-only button variant and `aria-label` requirement.
