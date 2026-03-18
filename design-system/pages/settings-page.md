# Settings / Configuration Page

<!-- Design system page: layout, components, and patterns for the settings view. AI-readable layout spec. -->

---

## Meta

```yaml
document_type: layout_spec
page_type: settings
scope: user_preferences | system_configuration
audience: ai_consumers | developers | designers
version: 1.0
```

---

## 1. Purpose

- **Primary**: Configure user or system preferences in a single, scannable place.
- **Secondary**: Persist changes, support reset/defaults, and surface validation before save.
- **Scope**: User preferences (theme, notifications, profile) and/or system configuration (API keys, integrations, feature flags) depending on product.

**Design intent**: Reduce cognitive load via clear grouping, predictable controls, and immediate feedback. Avoid long single-column forms; use sections and sidebar navigation.

---

## 2. Main Sections

### 2.1 Navigation Sidebar

| Property        | Value |
|----------------|-------|
| Placement      | Left; fixed or sticky on scroll |
| Width          | Narrow (e.g. 200–280px) or collapsible to icon-only |
| Role           | Jump to section; indicate active section |
| Content        | List of section labels (links or buttons) |
| Active state   | Highlight current section; optional subtle background/indicator |
| Responsive     | On small viewports: drawer/hamburger or top tabs instead of sidebar |

**Rules**:
- One item per logical section.
- Labels short and scannable (e.g. "Profile", "Notifications", "Security").
- Sidebar scrolls independently if section list is long.

### 2.2 Section Containers

| Property        | Value |
|----------------|-------|
| Structure      | Each section = one `<section>` or landmark with heading |
| Heading level  | `h2` for section title; `h3` for subsections |
| ID             | Each section has stable `id` for sidebar anchor links |
| Order          | Match sidebar order; same label text as sidebar item |

**Rules**:
- One primary heading per section.
- Subsections (e.g. "Email preferences", "Push notifications") use `h3` or equivalent.

### 2.3 Form Areas

| Property        | Value |
|----------------|-------|
| Location       | Inside section containers; main content column |
| Width          | Constrained max-width (e.g. 480–640px) for readability |
| Layout         | Vertical stack of fields; multi-column only when fields are short and related |

**Rules**:
- Group related fields (label + control) together.
- Use fieldsets for radio groups or logical clusters.
- Optional: card or panel per subsection for visual separation.

---

## 3. Layout Guidance

### 3.1 Form Alignment

| Aspect           | Rule |
|------------------|------|
| Label position   | Top-aligned above control (preferred for accessibility and narrow widths); or left-aligned with fixed label width for wide layouts |
| Control width    | Full width of form area; match width for related inputs in a row |
| Alignment        | Labels left-align; controls left-align (or full-width); action buttons align by convention (e.g. primary right, secondary left) |

### 3.2 Spacing

| Token / Use            | Rule |
|------------------------|------|
| Between sections       | Consistent vertical gap (e.g. 2–3rem or 32–48px) |
| Between subsection     | Medium gap (e.g. 1.5rem or 24px) |
| Between label and control | Small (e.g. 0.25–0.5rem or 4–8px) |
| Between form fields    | Medium (e.g. 1rem or 16px) |
| Internal padding       | Section/content padding consistent with app (e.g. 1.5rem) |

### 3.3 Section Hierarchy

- **Level 0**: Page title ("Settings" or "Configuration").
- **Level 1**: Sidebar items and section headings (e.g. Profile, Notifications).
- **Level 2**: Subsections within a section (e.g. "Email", "Push" under Notifications).
- **Level 3**: Optional sub-subsections or grouped fields with a label.

Use heading levels and spacing to reflect this hierarchy; do not skip levels.

---

## 4. Component Usage

### 4.1 Inputs

| Type           | Use case |
|----------------|----------|
| Text / email   | Single-line: name, email, URL, API key (masked if secret) |
| Textarea       | Multi-line: bio, description, webhook URL |
| Number         | Numeric only; pair with stepper or min/max when needed |
| Select / dropdown | Single choice from many; prefer when options > ~5 |
| Search/combobox | When list is long or searchable |

**Rules**: Always pair with visible label; use placeholder only as hint, not replacement for label. Support `aria-describedby` for hint or error text.

### 4.2 Toggles

- **Use**: Boolean on/off (e.g. "Enable notifications", "Dark mode").
- **Placement**: Label to the left of toggle; toggle right-aligned in row or end of row.
- **State**: Clearly show on/off (color and/or position); support focus and disabled.

### 4.3 Checkboxes

- **Use**: Multiple selections from a set; single optional (e.g. "I agree") or list of options.
- **Grouping**: Use fieldset + legend for groups; single checkbox can be standalone with label.
- **Layout**: Stack vertically for many options; horizontal only for 2–3 short labels.

### 4.4 Buttons

| Role           | Placement | Style |
|----------------|-----------|--------|
| Primary save   | End of form or sticky footer | Primary button |
| Secondary (Cancel / Reset) | Next to primary | Secondary or ghost |
| Destructive    | For "Reset to defaults" or delete; confirm before action | Destructive or secondary with confirmation |
| Inline actions | e.g. "Test connection" next to a field | Secondary or link |

**Rules**: One primary action per section or per page; avoid multiple primary buttons in one form.

### 4.5 Modals

- **Use**: Confirm destructive action; extra form (e.g. "Add integration"); critical warnings.
- **Content**: Title, body, primary + secondary actions; focus trap and close on Escape.
- **Avoid**: Burying main settings inside modals; prefer inline or dedicated route for complex flows.

---

## 5. Interaction Patterns

### 5.1 Collapsible Sections

- **When**: Sections or subsections that are optional or advanced; reduces initial scroll.
- **Behavior**: Click heading or dedicated control to expand/collapse; state (open/closed) can be persisted per user.
- **Affordance**: Chevron or arrow that rotates on open/close; optional "Advanced" or "More" label.
- **Accessibility**: `aria-expanded`; focus moves logically; keyboard toggle (Enter/Space on trigger).

### 5.2 Inline Validation

- **When**: On blur or on submit; avoid validating on every keystroke for free-text.
- **Display**: Error message below or next to control; error state on control (border/icon).
- **Rules**: One message per field; use `aria-invalid` and `aria-describedby` for the message.
- **Success**: Optional short success state (e.g. "Saved") without blocking the form.

### 5.3 Save / Persistence

- **Options**: Save per section; Save all at bottom; Auto-save on change (with debounce).
- **Feedback**: Loading state on submit; success toast or inline message; revert or error message on failure.
- **Dirty state**: Indicate unsaved changes (e.g. dot or label); confirm before navigate away if dirty.

---

## 6. Motion Rules (Expand / Collapse)

### 6.1 Principles

- **Purpose**: Clarify layout change (content appearing/disappearing); avoid distracting motion.
- **Respect reduced motion**: If `prefers-reduced-motion: reduce`, collapse/expand without animation or use very short duration (e.g. 0s or &lt; 100ms).

### 6.2 Expand / Collapse Animation

| Property     | Value |
|-------------|--------|
| Animated    | Height (or max-height) of content; optional opacity on content |
| Duration    | 150–250ms |
| Easing      | Ease-out for expand; ease-in for collapse (or symmetric ease-in-out) |
| Trigger     | Toggle control (chevron, "Show more", section header) |
| Content     | Do not animate nested expand/collapse simultaneously with parent; stagger or keep child closed until parent open |

### 6.3 Chevron / Icon

- Rotate icon (e.g. chevron) 0° → 90° (or 180°) on expand; reverse on collapse.
- Same duration and easing as content.
- Prefer transform (rotate) for performance.

### 6.4 Stagger (Optional)

- If multiple blocks expand (e.g. accordion), optional stagger delay 30–50ms per item.
- Do not stagger more than 3–4 items to avoid long total time.

---

## 7. Summary Checklist (AI / Implementation)

- [ ] Sidebar (or top nav on small screens) lists all sections; active section highlighted.
- [ ] Sections have stable IDs and headings; form areas constrained in width.
- [ ] Labels top-aligned (or left with fixed width); consistent spacing scale.
- [ ] Inputs, toggles, checkboxes, buttons used per usage table; modals for confirmations or extra flows.
- [ ] Collapsible sections use aria-expanded and keyboard support; inline validation on blur/submit with aria-invalid.
- [ ] Expand/collapse uses 150–250ms motion; reduced-motion respected; chevron rotates with content.
