# Motion Policy — AI-Readable

Motion supports clarity and feedback; respect `prefers-reduced-motion: reduce` (disable or shorten non-essential motion).

---

## Durations

| Context | Duration | Ease |
|--------|----------|------|
| UI feedback (hover, focus) | 100–150ms | ease |
| Layout / panel (modal, drawer) | 200–300ms | ease-out |
| Page / content enter | 150–250ms | ease |
| Alerts / toasts | 200ms | ease-out |

When `prefers-reduced-motion: reduce`: use 0ms or ≤50ms; no stagger or decorative motion.

---

## Component motion

| Component | Enter | Hover / state |
|-----------|--------|----------------|
| **Cards** | Slide in from top (e.g. translateY -12px → 0), 300ms | translateY(-2px), shadow 150–200ms |
| **Buttons** | — | Background/border transition 100–150ms |
| **Table row** | — | Background transition 150ms (button-hover on actions) |
| **Modal** | Overlay fade 150ms; panel scale/slide 200ms ease-out (modal-open) | — |
| **Alerts / toasts** | Slide + fade in from bottom, 200ms | Slide + fade out on dismiss |

---

## Naming (for implementation)

- **card-slide-in**: Cards on dashboard enter from top with stagger delay.
- **button-hover**: Row action buttons and primary/secondary buttons transition on hover.
- **modal-open**: Overlay fade-in + panel scale/slide.
- **alert-slide-fade**: Toast/alert slide up + fade in; reverse on dismiss.

---

*Reference: dashboard.md §7, data-page.md §Motion, accessibility.md §3.4.*
