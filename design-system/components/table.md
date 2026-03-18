# Table

## Purpose

Tables exist to present tabular data—rows and columns with clear headers—so users can scan, compare, and act on items. Use a table when the content is inherently tabular (e.g. device list, orders, comparison matrix); do not use a div-based grid for tabular data.

**When to use**: Data pages, settings lists (e.g. API keys, devices), reports, and any structured rows with consistent columns. Support optional row actions (Edit, Delete); Delete must open a confirmation modal before executing.

---

## Structure

- **Caption** (optional) — `<caption>` or `aria-label` on the table for context (e.g. "User devices").
- **Header row** — `<thead>` with `<th scope="col">` for each column. Optional sort controls (buttons with `aria-sort`).
- **Body** — `<tbody>` with `<tr>` and `<td>` per row. Optional row headers: `<th scope="row">` in first column.
- **Row actions** (optional) — Buttons (e.g. Edit, Delete) in a dedicated column; each with an accessible name (e.g. "Edit device", "Delete device").

---

## Variants

| Variant        | When to use |
|----------------|-------------|
| **Data table** | Standard rows of data; thead + tbody; `scope="col"` on each `<th>`. |
| **With actions** | Edit/Delete or other per-row actions in last column; buttons have clear `aria-label`. |
| **Sortable**   | Column headers are buttons or links; indicate sort direction with `aria-sort="ascending"` / `"descending"` / `"none"`. |
| **Striped**    | Alternate row background for readability; ensure contrast. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Default** | Rest background and border per tokens. |
| **Hover**   | Optional row highlight on hover. |
| **Focus**   | Action buttons in cells have visible focus ring (min 2px). Focus order: left to right, row by row. |
| **Selected** (optional) | If rows are selectable (e.g. checkboxes), show selected state clearly. |

---

## Layout & Spacing

- **Border**: 1px between cells or rows, or zebra stripes only; use border color from tokens.
- **Typography**: Cell content — `typography.sm` or `typography.base`; headers — `typography.titleS` or similar, bold.
- **Padding**: Consistent cell padding (e.g. 0.75rem). Reference spacing scale.
- **Colors**: Background and text from `design-system/tokens/color-primitives.json`; ensure contrast for text and borders.

---

## Accessibility

- **Semantics**: Use `<table>`, `<thead>`, `<tbody>`, `<th>`, `<td>`. Use `scope="col"` on column headers; `scope="row"` on row headers when applicable. Do not use div/grid for tabular data.
- **Caption/label**: Provide `<caption>` or `aria-label` on the table so screen readers get context.
- **Headers**: Column (and row) headers are announced with cell content; every cell is associated with its headers.
- **Actions**: Edit/Delete buttons are focusable and activatable with Enter/Space. Delete opens a confirmation modal (title and description) before executing.
- Reference: `.cursor/rules/accessibility.mdc` — Table (Section 4.3).

---

## Motion / Behavior

- Optional row hover transition; avoid animated sort that could disorient. Respect `prefers-reduced-motion` for any motion.

---

## Do's & Don'ts

- **Do** use `<th scope="col">` for column headers and semantic `<table>` structure.
- **Do** give row action buttons clear accessible names (e.g. "Edit device", "Delete device").
- **Do** use a confirmation modal for Delete; do not delete on single click.
- **Don't** use a div-based grid for tabular data; use a real table.
- **Don't** omit scope or headers so cells cannot be associated with headers.

---

## Example Usage (AI-Readable)

On a "Devices" page, render a table with caption "Registered devices". Columns: Name, Status, Last seen, Actions. Each header is `<th scope="col">`. Each row has a device name, status text, date, and in Actions two buttons: "Edit" with `aria-label="Edit device"` and "Delete" with `aria-label="Delete device"`. When the user clicks Delete, open a confirmation modal "Delete device?" with description "This action cannot be undone." and Confirm/Cancel. On confirm, remove the row and close the modal. Tab order moves left to right across cells, with Edit and Delete focusable in each row.
