# Agent instructions — Cakezerotest

Mandatory rules for any AI agent generating or editing UI in this codebase.

Read this document before writing any UI code.

---

## Quick decision tree

```

UI task received

  └─ Read design system first (required — see §Document index)

      └─ Component/pattern documented?

          ├─ YES → Use it exactly. Do not wrap, restyle, or extend its API.

          └─ NO  → FLAG a design system gap. Use closest primitive as fallback.

                   Comment: "Gap: no [X]; used [Y] — recommend adding [X]."

                   Never invent a new pattern silently.

Value needed (color / size / spacing / radius)?

  └─ Token exists? → Use token. Never hardcode.

  └─ No token?     → Use the scale defined in the component/page doc.

                     Still never use a magic number.

User-visible copy (labels, buttons, errors, headings)?

  └─ Read design-system/policies/voice-and-tone.md (sentence case, concise, clear errors).

```

---

## 1. Project overview

**What this is:** A design-system repository. It contains AI-readable documentation,

design tokens (DTCG-style JSON), component specs, policies, user intent flows, and

page layout specs. There is no application source code. This repo *is* the source of

truth for generating or validating UI.

**Stack:** Markdown docs, JSON tokens, Cursor rules `.cursor/rules/*.mdc`).

Figma is the design source; color primitives sync via MCP `get_variable_defs`).

Token extension for apps uses Tailwind `tailwind.config.cjs`).

**Agent goal:** Every piece of UI you generate must be token-driven, WCAG 2.2 AA–

compliant, and built only from documented components.

---

## 2. Document index

Read the relevant file(s) before acting. Each entry lists **when to read it**.

### Tokens

| File | When to read |

|---|---|

| `design-system/tokens/README.md` | First token lookup of any session |

| `design-system/tokens/typography.json` | Any font-family, size, weight, line-height, or letter-spacing value |

| `design-system/tokens/color-primitives.json` | Need a raw color value; verify a primitive exists |

| `design-system/tokens/color-semantic.json` | Any color usage in UI — always prefer semantic over primitive |

| `design-system/tokens/color-semantic.md` | Confirm contrast ratio (4.5:1 text / 3:1 UI) or semantic role meaning |

| `design-system/tokens/color-semantic-dark.json` | Semantic colors for dark theme / dark surfaces — same roles as light, different primitive mapping |

| `design-system/tokens/color-semantic-dark.md` | Dark theme contrast pairing, theme mapping (same CSS vars as light) |

| `design-system/tokens/spacing.json` | Any padding, margin, gap, or layout spacing value |

| `design-system/tokens/spacing.md` | How to apply spacing tokens, token table, and usage guidance |

### Components

| File | When to read |

|---|---|

| `design-system/components/button.md` | Any button, CTA, icon-only action, or destructive trigger |

| `design-system/components/card.md` | Any card, panel, or elevated surface |

| `design-system/components/chart.md` | Any data visualization |

| `design-system/components/dropdown.md` | Any dropdown, select, or menu |

| `design-system/components/input.md` | Any text field, textarea, or search input |

| `design-system/components/modal.md` | Any dialog, overlay, or confirmation panel |

| `design-system/components/sidebar.md` | Any sidebar, nav rail, or side panel |

| `design-system/components/table.md` | Any tabular data |

| `design-system/components/toggle.md` | Any toggle, switch, or on/off control |

### Policies

| File | When to read |

|---|---|

| `design-system/policies/accessibility.md` | Any interactive element; always read for agentic UI |

| `design-system/policies/iconography.md` | Any icon usage |

| `design-system/policies/motion.md` | Any animation, transition, or motion |

| `design-system/policies/typography.md` | Any text styling |

| `design-system/policies/tokens-and-css.md` | When implementing component specs; mapping tokens to CSS properties |

| `design-system/policies/voice-and-tone.md` | Any user-visible copy: labels, buttons, headings, errors, hints, toasts, empty states, aria text |

### Cursor rules

| File | When to read |

|---|---|

| `.cursor/rules/accessibility.mdc` | Generating or editing any interactive UI |

| `.cursor/rules/iconography.mdc` | Using any icon |

| `.cursor/rules/tokens-css.mdc` | Generating or editing UI that uses design tokens |

| `.cursor/rules/voice-and-tone.mdc` | Generating or editing user-visible UI copy |

### Intents

| File | When to read |

|---|---|

| `design-system/intents/README.md` | Unfamiliar with the intent system |

| `design-system/intents/confirmation.md` | Confirm/cancel flow |

| `design-system/intents/destructive-action.md` | Any irreversible action |

| `design-system/intents/form-submission.md` | Form submit and validation |

| `design-system/intents/navigation.md` | Moving between views |

| `design-system/intents/onboarding.md` | First-use or setup flows |

| `design-system/intents/outcome-feedback.md` | Success/failure feedback |

| `design-system/intents/_template-intent.md` | Adding a new intent |

### Pages

| File | When to read |

|---|---|

| `design-system/pages/dashboard.md` | Dashboard layout, grid, breakpoints |

| `design-system/pages/data-page.md` | Data-focused page patterns |

| `design-system/pages/forms-page.md` | Any form page (contact, registration, upload, filter) |

| `design-system/pages/settings-page.md` | Settings hierarchy and layout |

### Theme / global styles

No `variables.css`, `theme.ts`, or global stylesheet exists in this repo.

No Storybook config or `.mdx` stories exist.

Figma is the design source. Do not reference Figma variable names or pixel values

in code — use design-system token names and values only.

---

## 3. Before you write any UI

Execute these steps in order. Do not skip.

1. **Identify all components and patterns** required by the task.

2. **Look up each** in the document index (§2). Read the full file before using.

3. **Check for an intent** that covers the user flow (§2 → Intents).

4. **Identify all token values** you will need: color, typography, spacing, radius.

   - Color → `color-semantic.json` first; `color-primitives.json` only if no semantic token fits. For **dark theme** or dark surfaces, use `color-semantic-dark.json` / `color-semantic-dark.md` (same semantic roles and CSS variable names; swap value source by theme).

   - Typography → `typography.json`.

   - Spacing → `spacing.json` and `spacing.md` first; then the relevant component or page doc for layout-specific overrides.

   - Radius / z-index / breakpoints → read from the relevant component or page doc.

5. **User-visible copy** — For labels, buttons, headings, errors, hints, empty states, toasts, and screen-reader strings (`aria-label`, `aria-live`, etc.), read `design-system/policies/voice-and-tone.md` and apply tone, capitalization, and clarity rules.

6. **When implementing components**, ensure each spec property maps to the correct CSS property; read `design-system/policies/tokens-and-css.md` when turning specs into code.

7. **Check the gap rule** for anything not found (see §4).

8. **Write the UI** using only documented components and resolved token values.

---

## 4. Gap rule

If a required component, pattern, or token does not exist in the design system:

- **Do not improvise or invent** a new pattern.

- Use the closest existing primitive or component as a fallback.

- Add a comment to your code:

```

  // Gap: no [X] component. Using [Y] as fallback.

  // Recommend adding [X] to design-system/components/.

```

- Surface the gap explicitly in your response to the user.

---

## 5. Hard constraints

These apply to every line of UI code. Check output against this list before finishing.

- [ ] No hardcoded colors. All color values come from semantic tokens.

- [ ] No hardcoded font sizes, weights, families, or line-heights. All from `typography.json`.

- [ ] Each spec property is applied via the **correct** CSS property (e.g. font-size token sets `font-size`, not `color`).

- [ ] No token used with a utility that sets a different property (e.g. do not use `text-[var(--font-size-sm)]` for size; Tailwind `text-*` sets color).

- [ ] No magic-number spacing, radius, or z-index. All from documented scales.

- [ ] No third-party UI components when a design system component covers the use case.

- [ ] No modifications to a documented component's API or styles.

- [ ] No unnecessary wrappers or style overrides on design system components.

- [ ] No inline styles that bypass design tokens.

- [ ] No new design patterns without explicit instruction and a recorded gap comment.

- [ ] Icons use Material Design Icons only, per `iconography.md`.

- [ ] Motion respects `prefers-reduced-motion`, per `motion.md`.

- [ ] All interactive elements are WCAG 2.2 AA compliant, per `accessibility.md`.

- [ ] User-visible copy follows `voice-and-tone.md` (sentence case, concise buttons, actionable errors, consistent terms).

- [ ] Destructive actions follow the destructive-action intent flow.

---

## 6. File and folder conventions

| Content type | Location | Naming |

|---|---|---|

| Component docs | `design-system/components/` | `kebab-case.md` |

| Token files | `design-system/tokens/` | `kebab-case.json` / `.md` |

| Policy docs | `design-system/policies/` | `kebab-case.md` |

| Intent docs | `design-system/intents/` | `kebab-case.md`; new intents also added to `README.md` |

| Page docs | `design-system/pages/` | `kebab-case.md` |

| Cursor rules | `.cursor/rules/` | `kebab-case.mdc` |

**Headings and tables:** Sentence case. Token names: dot-notation (e.g. `semantic.primary.default`, `typography.fontSize.sm`).

---

## 7. When in doubt

Stop. Do not guess. Surface the ambiguity to the user and ask for clarification or

direction before proceeding. A wrong component choice or an invented pattern is harder

to undo than a short pause.