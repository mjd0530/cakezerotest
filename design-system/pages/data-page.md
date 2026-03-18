# Data Page — AI-Readable Layout Spec

<!-- Design system page: layout, components, and patterns for the data view. Use this doc when implementing or reasoning about data-focused pages. -->

---

## Purpose

- **Primary**: Viewing and interacting with structured data (tabular, list, or card-based).
- **Secondary**: Filtering, sorting, exporting, and drilling into records; optional bulk actions and inline editing.
- **Audience**: Users who need to scan, compare, and act on many rows/items; power users and analysts.
- **Success criteria**: Fast scan, predictable interactions, clear feedback, and accessible keyboard/screen-reader support.

---

## Key Sections

Sections appear in this order unless the spec says otherwise. Each is optional unless marked **required**.

| Section           | Required | Description |
|-------------------|----------|-------------|
| **Page header**   | Yes      | Title, optional description, primary actions (e.g. Add, Export, Refresh). |
| **Filters**       | No       | Controls to narrow the dataset (search, dropdowns, date range, tags). Collapsible on narrow viewports. |
| **Toolbar**       | No       | View toggle (table/grid/list), density, column visibility, bulk actions when selection exists. |
| **Table / List**  | Yes      | Main data surface: rows (or cards) and columns (or fields). |
| **Pagination**    | Conditional | Shown when total items exceed page size; prefer server-side for large datasets. |
| **Charts**        | No       | Summary or breakdown visuals; can be above the table or in a separate tab/panel. |
| **Detail panel**  | No       | Slide-out or inline panel for a single record; read-only or editable. |

**Section order (canonical)**:
1. Page header  
2. Filters (optional)  
3. Toolbar (optional)  
4. Charts (optional)  
5. Table / List (required)  
6. Pagination (when applicable)  
7. Detail panel (overlay or adjacent; not in main flow order)

---

## Layout Guidance

### Grid vs list

- **Table (grid)**: Default for many columns, sortable headers, comparison across rows. Use when columns are known and relatively stable.
- **List (cards/rows)**: Use when each item has rich content (e.g. image, summary, tags) or when column count is high and card layout improves scan. Prefer list on narrow viewports if table would horizontal-scroll heavily.
- **Grid (tile grid)**: Use for media or image-heavy items (e.g. assets, products with thumbnails). Less ideal for dense numeric/text comparison.

**Rule**: Prefer one primary view per page; view toggle (table/list/grid) only when both are explicitly supported.

### Column widths

- **Fixed**: Use for small, predictable content (e.g. status, icon, checkbox). Min width ~4rem; max as needed (e.g. 6rem for status).
- **Flex / proportional**: Use for text and numbers. Assign `min-width` so columns don’t collapse (e.g. 8rem for labels, 6rem for numbers).
- **Fill**: One column may take remaining space (e.g. “Name” or “Title”) so long as it has a reasonable max (e.g. 40ch or 30%).
- **Sticky**: First column (and optionally last) may be sticky for horizontal scroll; header row sticky on vertical scroll.

### Density

- **Compact**: More rows visible; use when users need high throughput (e.g. admin, exports). Row height ~2rem; padding reduced.
- **Default**: Balanced; row height ~2.75–3rem.
- **Comfortable**: More padding; use when content is long or readability is priority.

**Rule**: Persist density preference (e.g. in localStorage or user settings). Default to “default” if not set.

### Breakpoints

- **Narrow (< 640px)**: Prefer list/card view; filters in drawer or sheet; pagination compact (e.g. prev/next only).
- **Medium (640–1024px)**: Table with horizontal scroll or hide less important columns; filters can be inline or collapsible.
- **Wide (≥ 1024px)**: Full table; optional detail panel beside or overlay.

---

## Component Usage

### Tables

- **Semantic**: Use `<table>` with `<thead>`, `<tbody>`, `<th>` for headers. Scope `th` as `col` where appropriate.
- **Headers**: Support sort (click or key); show sort direction (arrow/icon) and consider “sort by” in label or aria.
- **Rows**: One row per record; support selection (checkbox or row click) per product decision.
- **Empty state**: Single message when no data (e.g. “No results”); distinct from loading and error.
- **Loading**: Skeleton rows or spinner; avoid layout shift (reserve height or use min-height).

### Pagination

- **Placement**: Below the table/list; full-width or right-aligned.
- **Controls**: Previous, Next, optional page numbers or “Page X of Y”. Optional: page size selector (10, 25, 50, 100).
- **Info**: “Showing X–Y of Z” or “Z results”; ensure it updates with filters/sort.
- **Behavior**: Page change must not scroll page to top unless desired; preserve scroll position or scroll to top of data area per spec.

### Modals / panels

- **Modal**: Use for destructive or high-focus actions (e.g. delete confirm, complex form). One at a time; trap focus; close on Escape and overlay click (if specified).
- **Detail panel**: Slide-out from right (or left); overlay on narrow; can be dismissible without committing. Use for view/edit of single record.
- **Sheets / drawers**: Use for filters or secondary content on small screens; same focus and escape rules as modals.

### Buttons

- **Primary**: One main action per context (e.g. “Add item”, “Save” in panel).
- **Secondary**: Outline or ghost for “Cancel”, “Clear filters”, “Reset”.
- **Destructive**: For delete/remove; use distinct style and confirm when irreversible.
- **Icon-only**: Use with `aria-label`; group in toolbar with visible labels on focus or in tooltip.

---

## Interaction Rules

### Sorting

- **Trigger**: Click header (or key activation if table is in tab order). Optional: dropdown “Sort by” in toolbar.
- **Cycle**: None → Ascending → Descending → (optional) None. Or Asc ↔ Desc only.
- **Feedback**: Icon + aria-sort on `<th>`; announce change to screen readers (live region or focus move).
- **Persistence**: Persist sort in URL or state so refresh and deep link keep same sort.

### Filtering

- **Apply**: On submit of filter form, or live (debounced) for single search box. Prefer “Apply” for multi-control filters to avoid constant requests.
- **Clear**: “Clear all” resets to defaults; individual controls can have their own clear.
- **Feedback**: Show active filter count or chips; reflect in “Showing X results” and empty state.
- **Persistence**: Persist in URL when possible so views are shareable.

### Selection

- **Row selection**: Checkbox per row; optional “select all” in header (for current page or all matching).
- **Bulk actions**: Toolbar appears when selection count > 0; actions (e.g. Delete, Export) apply to selected. Clear selection after action or on “Clear”.
- **Keyboard**: Space toggles focused row if row is focusable; checkbox must be focusable and operable by keyboard.

### Inline edits

- **When**: Only where explicitly specified (e.g. certain columns or “Edit” mode).
- **Save**: Per cell (blur or Enter) or per row (“Save row”). Cancel on Escape.
- **Validation**: Inline error under field or in toast; do not close edit until valid or cancelled.
- **Conflict**: If data changed remotely, show message and allow reload or overwrite per product rule.

---

## Accessibility Considerations

- **Landmarks**: Use `<main>` for main content; region/label for filters, table, and detail panel.
- **Headers**: One `<h1>` for page title; table has a title or caption (e.g. `<caption>` or `aria-labelledby`).
- **Table**: Prefer `<table>`; if using grid role, set `role="grid"`, `aria-rowcount`, `aria-colcount`, and cell roles.
- **Sort**: `aria-sort="ascending"|"descending"|"none"` on sortable `<th>`; expose sort state in header name if needed.
- **Pagination**: Label “Pagination” or “Table navigation”; current page indicated (e.g. `aria-current="page"`).
- **Focus**: Visible focus ring on all interactive elements; no focus trap except in modal/panel when open.
- **Live regions**: Use for “X results”, sort/filter result updates, and loading/error messages so screen readers get updates.
- **Color**: Do not rely on color alone for status or sort direction; use icon and text.
- **Keyboard**: Full operation without mouse (tab, Enter, Space, Escape, arrows where defined).

---

## Motion Rules for Updates and Transitions

- **Data refresh**: Prefer non-jarring update: keep scroll position; use subtle row highlight (e.g. brief background flash) for changed rows if needed. No full-page flash.
- **New rows**: If prepended/inserted, optional short slide-in or fade-in; duration ≤ 200ms. Avoid if it causes layout shift or distracts.
- **Removed rows**: Optional fade-out or collapse (≤ 200ms) before removal; or remove immediately and rely on pagination/empty state.
- **Sort/filter result**: Update table in place; optional light fade (e.g. 100ms) on table body. Do not animate each row unless list is very small.
- **Detail panel**: Open: slide in from side (200–300ms); close: reverse. Respect `prefers-reduced-motion`: skip or shorten to < 50ms.
- **Modals**: Fade overlay (100–150ms); scale or fade content (150–200ms). Respect reduced motion.
- **Loading**: Skeleton pulse or spinner only; no looping motion that can’t be disabled for reduced motion.
- **Rule**: If `prefers-reduced-motion: reduce`, set all transition durations to 0 or ≤ 50ms and avoid decorative motion.

---

## Summary Checklist for Implementation

- [ ] Page has single main purpose: viewing and interacting with structured data.
- [ ] Sections ordered: header → filters → toolbar → (charts) → table/list → pagination; detail panel overlay/side.
- [ ] Layout: table vs list vs grid chosen with column widths and density defined (and persisted where applicable).
- [ ] Table: semantic markup, sortable headers, loading and empty states, sticky header (and optional first column).
- [ ] Pagination: below data, with prev/next and optional page size; info text and URL/state persistence.
- [ ] Modals/panels: one primary use each; focus trap and Escape; detail panel for single-record view/edit.
- [ ] Sorting, filtering, selection, and inline edit rules followed with clear feedback and persistence where specified.
- [ ] Accessibility: landmarks, table caption/label, aria-sort, focus, live regions, keyboard-only operation, reduced motion.
- [ ] Motion: short, purposeful transitions; reduced-motion respected.
