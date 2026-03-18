# Intent: Navigation

## Purpose

The user intends to **move between views, sections, or resources** (e.g. go to a page, open a section, switch tabs). Navigation must be predictable, keyboard-accessible, and clearly indicated for the current location. Use semantic landmarks and labels so screen readers can identify regions.

**When to use**: Main app navigation (sidebar, nav bar), in-page section navigation (anchor links, tabs), breadcrumbs, pagination, and any link or control that changes the user's location or visible content.

---

## Flow

1. **Landmark** — Wrap navigation in `<nav>` with `aria-label` (e.g. "Main navigation", "Section navigation") so assistive tech can identify it.
2. **Items** — Use links (`<a href="...">`) for route changes or anchors; use buttons for in-page tabs or expand/collapse when the URL does not change. Ensure each item has an accessible name (visible text or `aria-label`).
3. **Current location** — Indicate the current page/section with `aria-current="page"` (for page-level nav) or `aria-current="true"` (for tabs/sections) and a visible highlight (e.g. background, border, or bold).
4. **Activation** — Links: default click and keyboard (Enter). Buttons (e.g. tabs): Enter and Space. Focus order follows visual order (top to bottom, left to right).
5. **After navigation** — On route change, focus the main content or a skip link target; avoid leaving focus on a stale trigger.

---

## Components to Use

| Element | Reference |
|--------|-----------|
| Sidebar / nav | `design-system/components/sidebar.md` — `<nav>`, `aria-label`, active state |
| Links & buttons | `components/button.md` — Use `<a>` when styled as button for navigation |
| Tables (pagination) | `design-system/pages/data-page.md` — "Table navigation" / pagination labels |
| Dashboard layout | `design-system/pages/dashboard.md` — Main nav, regions, `aria-label` |

---

## Accessibility

- **Landmark**: Use `<nav>` with `aria-label` so screen readers can jump to the region (e.g. "Main navigation").
- **Current page**: Active item has `aria-current="page"` (navigation) or `aria-current="true"` (tabs); combine with visible highlight so sighted users see where they are.
- **Keyboard**: All nav items focusable; activatable with Enter (links) or Enter/Space (buttons). No keyboard traps; focus order logical.
- **Focus ring**: Visible focus ring (min 2px) on `:focus-visible` for all nav items.

---

## Do's & Don'ts

- **Do** use `<nav>` and `aria-label` for navigation regions.
- **Do** mark the current page or tab with `aria-current` and a visible state.
- **Do** use `<a href="...">` for navigation that changes the URL; use buttons for in-page tabs or controls that don't change the URL.
- **Don't** use `<div>` or `<span>` with click handlers for navigation; use `<a>` or `<button>`.
- **Don't** change content without updating the active state or focus so users know where they are.

---

## Example Usage (AI-Readable)

- Main app sidebar: `<nav aria-label="Main navigation">` with list of links (Dashboard, Settings, Data). Current page link has `aria-current="page"` and a distinct background. Tab order follows list order; each link has visible text and focus ring.
- Settings page: Left sidebar with anchor links to sections (Profile, Notifications, Security). Each link has `href="#profile"` etc. Active section link has `aria-current="true"` and highlight. On click, scroll to section and optionally update active state from scroll position.
- Data table: Pagination controls with "Previous", "Next", and optional page numbers. Label "Pagination" or "Table navigation"; current page has `aria-current="page"`. Buttons/links focusable and activatable with keyboard.
