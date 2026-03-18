# Intent Name

<!-- One-line summary for tables and AI. -->

---

## Meta

```yaml
document_type: intent
intent_id: intent-name  # kebab-case
scope: user_goal | system_trigger
version: 1.0
```

---

## Purpose

Describe the user’s goal and why this intent exists.

---

## Trigger

Explain what user action or system event initiates this intent.

---

## Expected Outcome

Describe what should happen if the intent is successful.

---

## Required Components

List design system components (buttons, modals, alerts, inputs) to be used. Reference `design-system/components/*.md` where relevant.

| Component | Usage |
|-----------|-------|
| Button    | e.g. Primary CTA, destructive confirm |
| Modal     | e.g. Confirmation dialog |
| *(add rows)* | |

---

## Interaction Rules

- **Sequence of steps**: Ordered list of user/system steps.
- **Conditional behavior**: When to branch (e.g. validation failure, empty state).
- **Accessibility**: Per `design-system/policies/accessibility.md` — focus, labels, live regions, confirmations for destructive actions.

---

## Motion / Behavior Rules

Reference motion policy or `motion.md` if applicable. Respect `prefers-reduced-motion` for any animations.

---

## Do’s & Don’ts

Guidance for AI on what to avoid.

| Do | Don’t |
|----|--------|
| e.g. Use confirmation modal for destructive actions | e.g. Execute delete on single click |
| *(add rows)* | |

---

## Example AI Usage

Plain English description showing the intent in action.

> User clicks "Delete device". System opens a confirmation modal with title "Delete device?", description "This action cannot be undone.", and actions "Cancel" and "Delete". On "Delete", the device is removed and focus returns to the trigger; a status message is announced. On "Cancel" or Escape, the modal closes and focus returns to the trigger.
