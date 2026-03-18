# Toggle

## Purpose

Toggles exist to set a single on/off (boolean) value without a separate submit step—e.g. "Enable notifications", "Dark mode", "Public profile". Use a toggle when the choice is binary and the result can apply immediately or on save; use a checkbox when the user is selecting one or more items from a set or agreeing to a single statement.

**When to use**: Settings pages, feature flags, preferences, and any single yes/no or on/off control. Prefer toggles over two radio buttons for boolean settings.

---

## Structure

- **Label** — Visible text describing the setting (e.g. "Enable notifications"). Positioned left of the switch; associated with the control or referenced by `aria-labelledby`.
- **Switch** — Track (on/off background) and thumb (indicator). Controlled via `role="switch"` and `aria-checked`, or native `<input type="checkbox" role="switch">`.
- **Hint** (optional) — Short helper text; link via `aria-describedby`.

---

## Variants

| Variant     | When to use |
|-------------|-------------|
| **Default** | Single setting with label. Label left, switch right in a row. |
| **With hint**| Setting that needs clarification. Label and optional hint below or to the right; hint in `aria-describedby`. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Off**    | Thumb at start of track; track uses "off" background color. |
| **On**     | Thumb at end of track; track uses "on" background color. |
| **Hover**  | Optional subtle change on track or thumb. |
| **Focus**  | Visible focus ring (min 2px) on `:focus-visible` around the switch. |
| **Disabled** | Non-interactive; reduced opacity; not focusable; state still visible. |

---

## Layout & Spacing

- **Track**: Height and width from tokens; background colors for on/off that meet contrast (thumb and track distinguishable).
- **Thumb**: Contrast against track; position left (off) or right (on). Use tokens from `design-system/tokens/color-primitives.json` as needed.
- **Label**: `typography.base` or `typography.sm`.
- **Spacing**: Label and switch in one row with consistent gap (e.g. 0.75rem); optional margin below for hint.

---

## Accessibility

- **Role**: `role="switch"` with `aria-checked="true"` or `"false"`. Native option: `<input type="checkbox" role="switch">`.
- **Name**: Switch has an accessible name via visible label (associated or `aria-labelledby`) or `aria-label`. Do not rely on icon alone.
- **Keyboard**: Focusable; **Space** (and **Enter** where applicable) toggles the value. State announced (e.g. "On" / "Off").
- **Motion**: If thumb animates, respect `prefers-reduced-motion` (e.g. instant position change when reduced).

Reference: `.cursor/rules/accessibility.mdc` — keyboard and screen reader.

---

## Motion / Behavior

- Optional slide of thumb (e.g. 150–200ms) when toggling. Respect `prefers-reduced-motion`: use instant position change or minimal duration when reduced.

---

## Do's & Don'ts

- **Do** put the label to the left of the switch and keep the switch right-aligned in the row.
- **Do** ensure on/off is clear visually (position and/or color) and to assistive tech (`aria-checked`).
- **Don't** use a toggle for multiple selections; use checkboxes.
- **Don't** use an unlabeled switch; always provide an accessible name.

---

## Example Usage (AI-Readable)

On a settings page under "Notifications", add a row: label "Email notifications" on the left, toggle on the right. Associate the label with the switch so the switch is announced as "Email notifications, switch, on" or "off". When the user changes the value, persist it (e.g. on change or on Save). If the section has a hint like "We'll send you updates about your account", add it below the label and link it with `aria-describedby` on the switch. Ensure the toggle has a visible focus ring when focused via keyboard.
