# Sidebar

## Purpose

Sidebars exist to navigate between sections or routes and to show the current location. Use a sidebar when the app or page has multiple sections (e.g. settings: Profile, Notifications, Security) and users need a persistent, scannable list to jump between them.

**When to use**: App shell, settings pages, and any multi-section view. Prefer one item per logical section; keep labels short. On small viewports, use a drawer (overlay) or top tabs instead of a fixed sidebar.

---

## Structure

- **Container** — `<nav>` with `aria-label` (e.g. "Main navigation" or "Section navigation").
- **List** — Ordered or unordered list of links or buttons; each item has an accessible name (visible label or `aria-label` for icon-only).
- **Active indicator** — Current item has `aria-current="page"` or `"true"` and a visible highlight (background or border).
- **Expand/collapse control** (optional) — When sidebar is collapsible, a button with `aria-expanded` and accessible name (e.g. "Expand sidebar").

---

## Variants

| Variant     | When to use |
|-------------|-------------|
| **Full**    | Always show labels. Width ~200–280px. |
| **Collapsed** | Icon-only; expand on hover or click. Width ~48–64px. |
| **Responsive** | Full or collapsed on large screens; on small screens show as drawer (overlay) or replace with top tabs. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Default** | Rest background; items use default text color. |
| **Hover**   | Optional background or text change on item. |
| **Focus**   | Each link/button has visible focus ring (min 2px) on `:focus-visible`. |
| **Active**  | Current item has distinct background or border and `aria-current`. |
| **Expanded / collapsed** | Collapse control has `aria-expanded="true"` or `"false"`; when expanded, labels visible; when collapsed, icons only with accessible names. |

---

## Layout & Spacing

- **Width**: Full ~200–280px; collapsed ~48–64px. Use consistent token or fixed value.
- **Background**: Surface color from tokens (e.g. `design-system/tokens/color-primitives.json`).
- **Typography**: Labels — `typography.sm` or `typography.base`.
- **Spacing**: Consistent gap between items (e.g. 0.25rem or 0.5rem). Padding inside sidebar (e.g. 1rem).
- **Active state**: Background or border with sufficient contrast; do not rely on color alone.

---

## Accessibility

- **Landmark**: Use `<nav>` with `aria-label` so screen readers identify the region (e.g. "Section navigation").
- **Items**: Every link or button has an accessible name (visible label or `aria-label` for icon-only).
- **Current page**: Active item has `aria-current="page"` (for navigation) or `aria-current="true"` as appropriate; visible highlight.
- **Keyboard**: All items focusable; Tab order follows list order; activatable with Enter (and Space for buttons). No focus trap in the sidebar itself. If the sidebar is inside a drawer when open, trap focus within the drawer and return focus to the trigger (e.g. hamburger) on close.
- Reference: `.cursor/rules/accessibility.mdc` — keyboard, focus, semantics.

---

## Motion / Behavior

- Optional transition when expanding/collapsing (width or content fade). Respect `prefers-reduced-motion` (e.g. instant or very short).

---

## Do's & Don'ts

- **Do** use one item per section and keep labels short and scannable.
- **Do** indicate the active section clearly (visible state and `aria-current`).
- **Do** provide an accessible name for the nav and for every item (including icon-only when collapsed).
- **Don't** trap focus in the sidebar when it is always visible; only trap when it is a modal drawer.
- **Don't** use the sidebar for primary content; use it for navigation only.

---

## Example Usage (AI-Readable)

On a settings page, add a left sidebar with `aria-label="Settings sections"`. List three links: "Profile" (href="#profile"), "Notifications" (href="#notifications"), "Security" (href="#security"). When the user is in the Profile section, that link has `aria-current="page"` and a visible background or border. Tab order is Profile, Notifications, Security. On a small viewport, the sidebar is hidden and a "Menu" button opens a drawer containing the same links; when the drawer opens, focus moves to the first link and Tab cycles only within the drawer; Escape or selecting a link closes the drawer and returns focus to the Menu button. When the sidebar is collapsible, an "Expand sidebar" button toggles between icon-only and full labels and has `aria-expanded` set accordingly.
