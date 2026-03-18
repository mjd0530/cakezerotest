# Dropdown

## Purpose

Dropdowns exist to let users choose one option from a list (select) or pick an action from a set of commands (menu). Use a select when the user must pick a single value from a fixed or dynamic list; use a menu when the trigger opens a list of actions (e.g. "More options" with Edit, Duplicate, Delete).

**When to use**: Forms (country, category, status), toolbars (action menus), and filters. Prefer native `<select>` when the list is short and doesn't need search or custom styling; use a custom listbox or combobox when searchable or heavily styled. Use menu pattern for action lists.

---

## Structure

- **Trigger** — Button or input-like control that opens the list. Has `aria-haspopup` ("listbox" or "menu"), `aria-expanded`, and an accessible name (label or `aria-label`).
- **List** — Container with `role="listbox"` and `role="listbox"` options, or `role="menu"` and `role="menuitem"` for actions. Optionally search/filter input for combobox.
- **Options / items** — Each option has a visible label and is focusable; selected option has `aria-selected` (listbox) or equivalent.

---

## Variants

| Variant   | When to use |
|-----------|-------------|
| **Select** | Single choice from a list; form field. Native `<select>` or custom listbox with `aria-labelledby`, `aria-expanded`, `aria-haspopup="listbox"`. |
| **Combobox** | Searchable list; user can type to filter. Combobox + listbox pattern; type-ahead. |
| **Menu**   | Action list (e.g. "More" with Edit, Duplicate, Delete). Trigger button with `aria-haspopup="menu"`; list `role="menu"`; items `role="menuitem"`. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Closed** | Trigger visible; list hidden. |
| **Open**   | List visible; focus moves to first option or to search input (combobox). Arrow keys navigate; Enter/Space select. |
| **Hover**  | Option or menuitem highlighted. |
| **Focus**  | Trigger has focus ring when focused; when open, list item or search has focus with visible ring. |
| **Disabled** | Trigger disabled; does not open. |
| **Selected** (select/combobox) | Current value shown on trigger; option has `aria-selected="true"`. |

---

## Layout & Spacing

- **Trigger**: Same height as button or input (e.g. from spacing tokens); border, radius; padding for text and optional chevron.
- **List**: Surface background, shadow, border radius; max-height with scroll; padding for items. Use tokens from `design-system/tokens/color-primitives.json` for background and text.
- **Items**: Consistent padding (e.g. 0.5rem 0.75rem); hover/selected state. Typography: `typography.sm` or `typography.base`.

---

## Accessibility

- **Trigger**: Accessible name via visible label (select) or `aria-label` (e.g. "Open menu"). `aria-expanded="true"` when open, `"false"` when closed. `aria-haspopup="listbox"` or `"menu"`.
- **List**: Role `listbox` or `menu`; when listbox, options have `role="option"` and `aria-selected` where applicable. When menu, items have `role="menuitem"`.
- **Keyboard**: Arrow keys navigate; Enter/Space select or activate; **Escape** closes and returns focus to trigger. No trap: focus returns to trigger on close.
- **Destructive action**: If menu has "Delete", it opens a confirmation modal before executing.
- Reference: `.cursor/rules/accessibility.mdc` and ARIA APG for listbox, combobox, menu.

---

## Motion / Behavior

- Optional short open/close transition (e.g. 150–200ms fade or slide). Respect `prefers-reduced-motion`.

---

## Do's & Don'ts

- **Do** give the trigger an accessible name and set `aria-expanded` and `aria-haspopup` correctly.
- **Do** return focus to the trigger when the list closes (Escape or after selection).
- **Do** use confirmation modal for destructive menu items (e.g. Delete).
- **Don't** trap focus in the list; Escape must close and return focus to trigger.
- **Don't** use a select when the control is an action list; use the menu pattern.

---

## Example Usage (AI-Readable)

In a form, add a "Country" select: label "Country", trigger showing "Choose country" when empty or the selected country when set. On click or focus+Enter, open a listbox of countries. User selects with Arrow keys and Enter, or clicks an option; list closes and focus returns to the trigger. In a table row, add a "More options" button that opens a menu with "Edit", "Duplicate", "Delete". Each item is a menuitem. When the user chooses "Delete", open a confirmation modal "Delete this item?" with description and Confirm/Cancel; on confirm, perform delete and close modal. When the menu closes (Escape or after click), return focus to the "More options" button.
