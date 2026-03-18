# Intents

<!-- Design system: user intents. Documents goals, triggers, outcomes, and interaction rules for AI and implementers. -->

---

## What are intents?

An **intent** describes a user’s goal and how the system should fulfill it: what triggers it, what happens when it succeeds, which components to use, and how to behave (sequence, conditionals, accessibility, motion).

Intents are used to:

- Keep AI-generated flows consistent with the design system
- Align components, policies (accessibility, iconography), and pages
- Provide clear Do’s & Don’ts and example usage

---

## How to add an intent

1. Copy `_template-intent.md` and rename it (e.g. `delete-item.md`, `save-settings.md`).
2. Fill in every section; remove or mark N/A only when truly not applicable.
3. Cross-reference components (`components/*.md`), policies (`policies/*.md`), and pages (`pages/*.md`) as needed.
4. In **Motion / Behavior Rules**, reference the motion policy or `motion.md` when it exists.

---

## Intent index

| Intent | File | Purpose (one line) |
|--------|------|--------------------|
| Destructive Action | `destructive-action.md` | Handle irreversible or critical actions (e.g. delete) with confirmation modal, focus trap, and accessible feedback. |
| Confirmation | `confirmation.md` | Explicit confirm or cancel before proceeding; destructive variant see destructive-action. |
| Navigation | `navigation.md` | Move between views, sections, or resources with predictable, keyboard-accessible nav. |
| Onboarding | `onboarding.md` | Guide new users through setup or first-use flows. |
| Form submission | `form-submission.md` | Submit form data with validation and feedback. |
| Outcome feedback (confirmation) | `outcome-feedback.md` | Confirm success or failure after form submit or significant non-destructive action (modal or toast, alerts). |

---

## Related

- **Components**: `design-system/components/` — buttons, modals, inputs, etc.
- **Policies**: `design-system/policies/` — accessibility, iconography, typography
- **Pages**: `design-system/pages/` — dashboard, settings, data page layouts
