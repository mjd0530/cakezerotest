# Dashboard Page — AI-Readable Layout Document

<!-- Design system page: layout, components, and patterns for the dashboard. Format optimized for AI consumption: structured sections, unambiguous keys, consistent terminology. -->

---

## 1. Purpose and User Goals

**page_id:** `dashboard`
**page_type:** application_hub

**purpose:**
- Single entry point for at-a-glance status and key metrics.
- Orient the user and surface actionable items (alerts, tasks, quick links).
- Support decision-making via summarized data and navigation to detail views.

**user_goals:**
- Understand current state (e.g., system health, counts, recent activity).
- Identify what needs attention (alerts, overdue items).
- Reach deeper sections (reports, settings, lists) in one or two clicks.
- Optionally customize what appears (widgets, order, density).

**success_criteria:**
- Primary metrics visible without scroll on standard viewport.
- Critical alerts always visible or clearly indicated.
- Navigation to any major area in ≤2 clicks from dashboard.

---

## 2. Main Regions

Regions are listed in DOM order. Each has a stable `region_id` for references.

| region_id     | role        | placement   | contains |
|---------------|-------------|-------------|----------|
| `header`      | global chrome | top, full width | logo, primary nav, user menu, global actions |
| `nav`         | primary navigation | left (desktop) / drawer (mobile) | main app sections, active state |
| `main`        | primary content | center, flexible | dashboard widgets, cards, charts, alerts |
| `sidebar`     | secondary / context | right (desktop) or collapsible | filters, quick actions, recent activity, tips |
| `footer`      | meta / legal | bottom, full width | links, version, legal |

**region_visibility:**
- `header`: always visible.
- `nav`: always on desktop; on mobile, toggleable (drawer/overlay).
- `main`: always visible; scrollable when content overflows.
- `sidebar`: visible on desktop when not collapsed; on mobile, optional drawer or below main.
- `footer`: visible when in view; can be minimal (e.g., single line).

**semantic_landmarks:**
- Use `<header>`, `<nav>`, `<main>`, `<aside>` (sidebar), `<footer>`.
- Each major region has `role` and `aria-label` where beneficial (e.g. `aria-label="Main navigation"`).

---

## 3. Layout Rules

### 3.1 Grid

**grid_system:** CSS Grid for page shell; grid or flex for widget/card layout inside main.

**page_grid:**
- Desktop (≥1024px): 12 columns; gutter 24px; margin 24px (or 32px) each side.
- Tablet (768px–1023px): 8 columns; gutter 16px; margin 16px.
- Mobile (<768px): 4 columns or single column; gutter 16px; margin 16px.

**main_content_grid:**
- Main content area uses same column count as page (12 / 8 / 4).
- Widgets span columns via explicit `grid-column` or span classes (e.g. span-6, span-12).

### 3.2 Spacing

**scale (base 4px):**
- xs: 4px
- sm: 8px
- md: 16px
- lg: 24px
- xl: 32px
- xxl: 48px

**region_spacing:**
- Between regions (e.g. header to main): `lg` or `xl`.
- Between widgets inside main: `md` or `lg`.
- Internal padding of header/footer: `md` vertical, `lg` horizontal.
- Card internal padding: `md`; between cards in a row: `md`.

### 3.3 Column Widths and Spanning

**desktop:**
- Header: full width (12/12).
- Nav: fixed or min width 240px; main + sidebar fill remainder.
- Main: flexible; suggest 8/12 or 9/12 when sidebar present; 12/12 when sidebar hidden.
- Sidebar: 3/12 or 4/12; min-width 280px, max-width 360px.
- Footer: full width (12/12).

**widget_spanning:**
- Hero/KPI strip: full width (12/12).
- Large chart or table: 8/12 or 12/12.
- Medium card: 4/12 or 6/12.
- Small card or compact widget: 3/12 or 4/12.
- Define breakpoints where widgets reflow (e.g. 2-column → 1-column at 768px).

### 3.4 Responsive Behavior

**breakpoints:**
- sm: 0–767px
- md: 768–1023px
- lg: 1024–1279px
- xl: ≥1280px

**rules:**
- Below `md`: single column for main; nav and sidebar become overlay/drawer; header may compact (e.g. icon-only nav).
- At `md`: main can show 2 columns for widgets; sidebar optional or below main.
- At `lg` and above: full layout with nav + main + sidebar; multi-column widgets.

**containment:**
- Main and sidebar scroll independently if desired; or single page scroll. Prefer one consistent behavior (e.g. single scroll) unless spec says otherwise.
- Max content width optional (e.g. 1440px) with centering for very large screens.

---

## 4. Component Usage

### 4.1 By Region

**header:**
- Logo (link to dashboard/home).
- Primary nav: links or tabs (Dashboard, Reports, Settings, etc.).
- Buttons: primary CTA (e.g. "New"), secondary actions.
- User menu: avatar + dropdown (profile, preferences, sign out).
- Optional: global search, notifications icon with badge.

**nav:**
- List of nav items (icons + labels on desktop; icons only when collapsed).
- Active state: distinct background and/or border.
- Optional: expand/collapse of nav bar; nested sections (accordion).

**main:**
- Alert banner (if any): full-width at top of main.
- Optional welcome/date line.
- KPI cards (counts, percentages, short trend).
- Charts (bar, line, donut as specified).
- Data tables or lists (compact; link to full view).
- Cards for discrete units (e.g. "Recent activity", "Quick actions").
- Empty state when no data: illustration + message + CTA.

**sidebar:**
- Filters (e.g. date range, segment).
- Short list (e.g. "Recent", "Favorites").
- Secondary actions or tips.
- Optional: collapse to icons only.

**footer:**
- Text links (Help, Terms, Privacy).
- Version or environment label.
- Optional: compact sitemap.

### 4.2 Component Inventory

| component   | use_case | variant / notes |
|------------|----------|------------------|
| card       | KPI, chart container, list container | default, elevated, bordered |
| chart      | trends, distribution, comparison | line, bar, donut, area; specify per widget |
| button     | actions | primary, secondary, ghost, danger |
| alert      | system or inline message | info, success, warning, error; dismissible |
| badge      | counts, status | numeric, dot, label |
| avatar     | user identity | with or without dropdown |
| nav_item   | section switch | default, active |
| table      | tabular data in main | compact, striped, sortable |
| list       | simple items in sidebar | default, with icons |
| modal/drawer | detail or form | use for secondary flows, not primary dashboard content |

### 4.3 Naming and Placement

- Use design-system component names consistently (e.g. `Card`, `Button`, `Alert`).
- Prefer composition: e.g. `Card` wraps `Chart` or `Table`; `Alert` sits above card grid.
- Document which slots (e.g. "top-left KPI") use which component in a separate widget map if needed.

---

## 5. Hierarchy Guidance

### 5.1 Primary vs Secondary

**primary:**
- Content that directly supports the main user goals: KPIs, critical alerts, main chart or table.
- Placed in `main`, above the fold when possible.
- Higher visual weight: larger type, stronger color, more contrast.

**secondary:**
- Supporting content: filters, recent activity, tips, secondary metrics.
- Placed in `sidebar` or below primary in `main`.
- Lower visual weight: smaller type, muted color, less prominent borders/backgrounds.

**tertiary:**
- Meta: footer links, version, non-critical hints.
- Lowest visual weight; do not compete with primary/secondary.

### 5.2 Visual Hierarchy Rules

- One clear focal area per screen (e.g. primary KPI or primary chart).
- Headings: one `h1` per page (e.g. "Dashboard" or user-specific title); then `h2` for regions, `h3` for card titles.
- Contrast: primary elements meet WCAG AA for text and interactive elements.
- Density: allow optional "compact" mode where spacing and font size reduce consistently.

### 5.3 Information Priority

**order of importance (top to bottom / left to right in LTR):**
1. Critical alerts (always first if present).
2. Primary KPIs (e.g. row of 3–5 metrics).
3. Main chart or main table.
4. Secondary widgets (e.g. two columns of cards).
5. Sidebar content.
6. Footer.

---

## 6. Interaction Patterns

### 6.1 Hover

- Interactive elements (buttons, links, nav items, table rows) have visible hover state (background, border, or underline).
- Cards that are clickable have hover state (elevation or border).
- No hover-only critical information; hover can reveal tooltips or secondary actions.

### 6.2 Focus

- All interactive elements focusable in a logical order (tab order follows layout).
- Visible focus ring (2px outline or box-shadow) that meets contrast requirements.
- Skip link optional: "Skip to main content" at top of page.

### 6.3 Expand / Collapse

- Sidebar: can collapse to icon-only; state persisted (e.g. in localStorage).
- Nav: on mobile, expands from drawer; closing on selection or overlay click.
- Cards: optional "Expand" to modal or full-page view for detail.
- Accordions in nav or sidebar: one or multiple sections open; state clear (icon rotation or chevron).

### 6.4 Selection and Multi-Action

- Tables/lists: optional row selection (checkbox) for bulk actions; toolbar appears when selection non-empty.
- Single-click: navigate or open detail; double-click only if explicitly specified.

### 6.5 Loading and Empty States

- Initial load: skeleton placeholders for main widgets (cards, chart, table) with same layout as content.
- Refresh: optional subtle indicator (e.g. spinner in header or on widget) without blocking whole page.
- Empty state: illustration + short message + CTA (e.g. "Add your first report"); no blank areas.

---

## 7. Motion and Transitions

### 7.1 Principles

- Motion supports clarity and feedback, not decoration.
- Respect `prefers-reduced-motion`: when set, disable or shorten non-essential motion.
- Keep durations short (100–300ms for UI feedback; up to 400ms for layout changes).

### 7.2 Page and Region Transitions

- **Page load:** Optional light fade-in of main content (e.g. 150–200ms) after shell is painted.
- **Nav/sidebar open/close:** 200–300ms ease; slide + optional opacity for overlay.
- **Route change (SPA):** Optional crossfade or short fade (150–250ms) for main content only.

### 7.3 Component-Level Motion

- **Buttons / links:** Quick opacity or background transition on hover/focus (100–150ms).
- **Cards:** Hover elevation/transform (e.g. translateY(-2px)) in 150ms ease.
- **Modals / drawers:** Backdrop fade 150ms; panel slide 250ms ease-out.
- **Alerts:** Slide-in or fade-in on appear; optional slide-out on dismiss (200ms).
- **Charts:** Optional initial animate-on-load (e.g. 400–600ms) for bars/lines; no continuous animation unless live data.

### 7.4 Stagger and List Updates

- **Widgets on load:** Optional stagger (50–80ms delay between each) for first paint only.
- **List/table updates:** New rows can fade or slide in (150–200ms); removed rows fade/slide out before DOM removal.
- **Counters (KPI):** Optional short number increment animation (200–400ms) on value change.

### 7.5 Reduced Motion

- If `prefers-reduced-motion: reduce`:
  - Use instant or very short (≤50ms) transitions.
  - Disable stagger, decorative animations, and parallax.
  - Preserve essential feedback (e.g. focus ring, state change) without motion where possible.

---

## 8. Reference Summary (Quick Parse)

```yaml
page: dashboard
regions: [header, nav, main, sidebar, footer]
grid: 12/8/4 columns by breakpoint; 24/16/16 gutter
spacing_scale: [4, 8, 16, 24, 32, 48] # px
breakpoints: { sm: 768, md: 768, lg: 1024, xl: 1280 }
primary_components: [card, chart, button, alert, table]
hierarchy: alerts > KPIs > main chart/table > secondary widgets > sidebar > footer
motion_duration: 100-300ms UI; up to 400ms layout; respect prefers-reduced-motion
```

---

*End of dashboard layout document. Use section numbers and keys (e.g. region_id, component, breakpoints) when referring to this spec in implementation or prompts.*
