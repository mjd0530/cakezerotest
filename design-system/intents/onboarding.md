# Intent: Onboarding

## Purpose

The user is **new to the product or a feature** and needs guided setup, education, or first-time configuration. Onboarding can be a short welcome flow, a multi-step wizard, or contextual hints. The user must be able to skip or dismiss where appropriate, and progress must be clear and interruptible.

**When to use**: First launch, new feature introduction, setup wizards (e.g. connect account, choose preferences), and optional tours or tooltips. Do not block critical tasks with non-skippable onboarding.

---

## Flow

1. **Entry** — Onboarding starts from first launch, a "Get started" entry point, or an optional "Take a tour" control. Avoid auto-launching long flows without user initiation when possible; prefer a clear entry (e.g. "Set up your account" or "Take a 3-step tour").
2. **Steps** — Present one concept or step per screen (or per modal/drawer step). Use progress indicator (e.g. "Step 2 of 4") with `role="progressbar"` or `aria-label` so assistive tech announces progress.
3. **Actions** — Each step has a primary action (e.g. "Next", "Finish", "Get started") and optionally "Skip" or "Back". "Skip" must be visible and keyboard-accessible; avoid hiding skip in a menu.
4. **Completion** — On finish or skip, close the flow and either persist "onboarding completed" state or allow the user to re-enter later. Restore focus to the main content or the control that opened the flow.
5. **Interruptibility** — Long-running or multi-step flows should allow cancel/skip (see `design-system/policies/accessibility.md` — Interruptibility). Announce step changes (e.g. live region: "Step 2 of 4: Connect your account").

---

## Components to Use

| Element | Reference |
|--------|-----------|
| Modals / steps | `components/modal.md` — Form or informational variant for each step |
| Buttons | `components/button.md` — Primary (Next/Finish), Secondary (Back/Skip) |
| Progress | `design-system/policies/accessibility.md` — Progress bar / status (determinate or indeterminate) |
| Focus & motion | `design-system/policies/accessibility.md` — Focus trap in modal; restore on close; respect `prefers-reduced-motion` |

---

## Accessibility

- **Progress**: Use `role="progressbar"` with `aria-valuenow`, `aria-valuemin`, `aria-valuemax` when steps are determinate, or `role="status"` with `aria-live="polite"` and a label like "Step 2 of 4".
- **Step content**: Each step has a clear heading and short description; use `aria-labelledby` and `aria-describedby` if inside a dialog.
- **Skip / Cancel**: Visible and focusable; accessible name (e.g. "Skip tour", "Cancel"). Do not trap users in a long flow without a way out.
- **Focus**: When moving to the next step, move focus to the step content or primary action; when closing, return focus to the trigger or main content.
- **Motion**: Any step transition or decorative animation must respect `prefers-reduced-motion`.

---

## Do's & Don'ts

- **Do** provide a skip or cancel option for multi-step onboarding.
- **Do** show progress (e.g. "Step 2 of 4") and announce step changes for screen readers.
- **Do** use one primary action per step; keep steps short and scannable.
- **Don't** auto-start long, non-skippable flows that block the main task without clear user initiation.
- **Don't** rely on motion or animation alone to convey progress; use text or progress indicator.

---

## Example Usage (AI-Readable)

- First-launch wizard: Modal "Welcome" with body "Connect your account to get started." Buttons: "Skip for now" (secondary), "Get started" (primary). On Get started, go to step 2: "Connect account" with form or OAuth; progress text "Step 2 of 3". On Skip, close modal and set "onboarding skipped"; focus returns to main content.
- Optional tour: User clicks "Take a tour". Open first step with title "Dashboard overview", description "Here you can see your key metrics." Buttons: "Skip tour", "Next". Each step updates progress "Step 1 of 4"; on last step "Finish" closes and returns focus. Escape or Skip closes at any time and restores focus.
