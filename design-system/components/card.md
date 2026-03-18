# Card

## Purpose

Cards exist to group related content (text, media, controls) in a single container so users can scan and act on discrete chunks. Use a card when you need visual separation or a clickable unit (e.g. dashboard tile, settings section, list item).

**When to use**: Dashboards, settings sections, feature lists, content previews, or any place where a bordered or elevated block improves scannability. Use an interactive card when the whole block should link or navigate.

---

## Structure

- **Header** (optional) — Title and optional subtitle; heading level matches page hierarchy (e.g. `h3` inside a section of `h2`s).
- **Body** — Main content (text, list, media, or form controls).
- **Footer / actions** (optional) — Buttons or links (e.g. "View more", "Edit").

When the card is interactive, the whole card is one focusable wrapper (`<a>` or `<button>`) with an accessible name that describes the destination or action.

---

## Variants

| Variant       | When to use |
|---------------|-------------|
| **Default / bordered** | General grouping with subtle border and background. |
| **Elevated**  | Higher emphasis (e.g. dashboard highlight); use shadow instead of or in addition to border. |
| **Interactive** | Entire card is clickable (e.g. navigates to detail). Same structure; wrapper has href or button role and full `aria-label`. |

---

## Interaction States

| State    | Behavior |
|----------|----------|
| **Default** | Rest style per variant (border or shadow). |
| **Hover**   | Only for interactive cards: subtle background or border change. |
| **Focus**   | Visible focus ring (min 2px) on wrapper when card is interactive; inner buttons/links have their own focus. |
| **Selected** (optional) | If card can be selected (e.g. in a list), show selected state clearly. |

---

## Layout & Spacing

- **Background**: Surface color from tokens (e.g. `primitive.common.white` or semantic surface).
- **Border**: 1px solid border color; radius (e.g. 0.5rem). Omit or reduce for elevated variant if using shadow.
- **Shadow**: Use elevation token for elevated variant.
- **Padding**: Consistent scale (e.g. 1rem or 1.5rem); use tight/default/relaxed options if defined in tokens.
- **Typography**: Title — `typography.headlineS` or `typography.titleM`; body — `typography.base`.

---

## Accessibility

- Use `<article>` or `<section>` when the card represents a self-contained unit; otherwise `<div>` with a class.
- **Interactive card**: Single focusable wrapper with an accessible name (e.g. "View report: Q4 2024"); visible focus ring; activatable with Enter and Space.
- Buttons and links inside the card must have clear accessible names. Destructive actions in cards must use a confirmation modal.

---

## Motion / Behavior

- Optional hover transition (e.g. 150–200ms) for background or shadow on interactive cards. Respect `prefers-reduced-motion`.

---

## Do's & Don'ts

- **Do** give interactive cards a single focusable wrapper and a descriptive accessible name.
- **Do** keep heading levels consistent with the page (e.g. card title as `h3` under section `h2`).
- **Do** use cards for grouping; use lists or tables when the content is strictly tabular or list-based.
- **Don't** nest multiple focusable wrappers (e.g. a link card with a link in the body that goes elsewhere is fine; avoid card-as-link plus card-as-button).
- **Don't** rely on color alone for borders/shadows; ensure sufficient contrast for borders and text.

---

## Example Usage (AI-Readable)

On a dashboard, show three cards in a grid: "Revenue", "Users", and "Orders". Each card has a title, a short summary or number, and an optional "View report" link in the footer. The "View report" link is the only focusable element in the card; the card itself is not clickable. In a settings page, use a bordered card for "Profile" with a header "Profile", body with name and email fields, and footer with "Save" and "Cancel" buttons. For a list of articles, use an interactive card: the whole card is a link to the article; the wrapper has `aria-label="Read: [Article title]"` and a visible focus ring when focused.
