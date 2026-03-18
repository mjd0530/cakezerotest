# Chart

## Purpose

Charts exist to visualize numeric or series data (trends, comparisons, proportions) so users can quickly see patterns. Use a chart to supplement, not replace, the underlying data—ensure key information is also available as text or a table for screen readers and when motion is reduced.

**When to use**: Dashboards, reports, and analytics views. Choose type by data: line for trends over time, bar for category comparison, pie/donut for proportions (few segments), area for cumulative or volume.

---

## Structure

- **Title** — Visible heading (e.g. `h3`) or region with `aria-labelledby`; required for context.
- **Description / summary** — Short text with key takeaway or data (e.g. "Q4 total $45k; peak in November"). Ensures content is perceivable without the graphic.
- **Graphic** — Canvas or SVG with `role="img"` and `aria-label` or `aria-labelledby` describing the chart (e.g. "Line chart of revenue January to December 2024").
- **Legend** (optional) — Labels for series; do not rely on color alone; use pattern or text.

---

## Variants

| Type       | When to use |
|------------|-------------|
| **Line**   | Trends over time; one or more series. |
| **Bar**    | Compare categories (vertical or horizontal). |
| **Pie / Donut** | Proportions; use few segments; avoid many slices. |
| **Area**   | Cumulative or volume; stacked or single series. |

---

## Interaction States

| State      | Behavior |
|------------|----------|
| **Default** | Rendered chart; static or minimal motion. |
| **Hover**   | Optional tooltip or highlight on data point/series; ensure info is also available in summary or table. |
| **Focus**   | If chart or parts are focusable (e.g. for drill-down), visible focus ring and keyboard support. |
| **Reduced motion** | When `prefers-reduced-motion: reduce`, avoid decorative animation; use static or simplified view. |

---

## Layout & Spacing

- **Title**: `typography.headlineS` or `typography.titleM`.
- **Axis labels / legend**: `typography.titleS` or `typography.sm`.
- **Colors**: Use design tokens for series (from `design-system/tokens/color-primitives.json`); ensure sufficient contrast and that series are distinguishable (not by color alone—use pattern, label, or legend).
- **Grid / lines**: Subtle stroke color. Consistent padding around chart area.

---

## Accessibility

- **Name**: Chart graphic has an accessible name via `role="img"` and `aria-label` or `aria-labelledby` with a concise description (e.g. "Bar chart: Q1–Q4 revenue in thousands").
- **Summary**: Provide a text summary or table of key data so the information is available without the graphic.
- **Color**: Do not rely on color alone to distinguish series; use pattern, label, or legend.
- **Motion**: Respect `prefers-reduced-motion`; avoid decorative animation.
- Reference: `.cursor/rules/accessibility.mdc` — perceivable content, motion.

---

## Motion / Behavior

- Optional subtle animation on initial load (e.g. draw or fade); keep short. When `prefers-reduced-motion: reduce`, use static or minimal motion (e.g. no animated draw).

---

## Do's & Don'ts

- **Do** provide a visible title and a short text summary or key data table.
- **Do** give the chart graphic `role="img"` and an accessible name.
- **Do** make series distinguishable by more than color (pattern, legend, labels).
- **Don't** rely on the chart alone to convey critical data; duplicate key info in text.
- **Don't** use decorative animation when reduced motion is preferred.

---

## Example Usage (AI-Readable)

On a dashboard, add a "Revenue by month" chart. Use a visible title "Revenue by month" and a short description: "Q4 total $45k; peak in November." The chart graphic (canvas or SVG) has `role="img"` and `aria-label="Line chart of revenue January to December 2024"`. Use token colors for the line and axis; ensure contrast. If the user has reduced motion enabled, show the chart without an animated draw. Optionally provide a compact table of month–value below or in a details region for full data access.
