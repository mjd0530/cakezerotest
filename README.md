# Cakezerotest — Design system (AI-ready)

Design-system repository: tokens, component specs, policies, intents, and page patterns. No app code — this repo is the **source of truth** for generating or validating UI. Intended for use by **development teams and AI coding tools** (e.g. Cursor, Copilot, Codex) when building interfaces.

---

## What’s in this repo

| Path | Contents |
|------|----------|
| `design-system/tokens/` | Color (semantic + primitives), typography (JSON + docs) |
| `design-system/components/` | Button, card, modal, table, input, dropdown, sidebar, etc. |
| `design-system/policies/` | Accessibility, iconography, motion, typography |
| `design-system/intents/` | Confirmation, destructive action, form submission, navigation, etc. |
| `design-system/pages/` | Dashboard, data page, forms page, settings page |
| `.cursor/rules/` | Cursor rule files (accessibility, iconography) |
| `AGENTS.md` | **Entry point for AI agents** — document index and mandatory rules |

---

## Using this repo with AI tools

### Option 1: Clone and open in Cursor

1. Clone the repo and open the folder in Cursor.
2. Cursor will pick up `.cursor/rules/*.mdc` and `AGENTS.md` for the workspace.
3. When building UI in **another** app repo, add this repo as a **reference**: open both folders in the same workspace, or use Cursor’s “Add folder to workspace” and point to the cloned path. Then instruct the AI to follow the design system in cakezerotest (e.g. “Use the design system in cakezerotest — read AGENTS.md first”).

### Option 2: Point AI at the repo URL

In Cursor (or similar), you can reference this repo by URL in the prompt, for example:

- “Follow the design system at **https://github.com/mjd0530/cakezerotest** — read `AGENTS.md` and the document index before generating UI.”
- For Cursor: add the repo as a **linked folder** or **@-reference** if your setup supports it, so the model can read files from it.

### Option 3: Copy rules into your app repo

- Copy `AGENTS.md` and `.cursor/rules/` into your application repo.
- Copy or symlink the `design-system/` directory so tokens and docs are available.
- Ensure your AI instructions tell the agent to read `AGENTS.md` and the document index before writing UI.

### Entry point for agents

**AI agents should start here:** [AGENTS.md](./AGENTS.md). It contains the document index (when to read which file), the gap rule, and hard constraints (tokens only, no hardcoded values, WCAG 2.2 AA, etc.).

---

## For maintainers

- **Figma** is the design source; color primitives can sync via MCP `get_variable_defs`.
- **Token extension** for apps is typically done via Tailwind (e.g. `tailwind.config.cjs`).
- All UI generated from this system must be token-driven, use only documented components, and comply with the accessibility and iconography policies.

---

## Publish to GitHub

This repo lives at **https://github.com/mjd0530/cakezerotest**. To clone or add as remote:

```bash
git clone https://github.com/mjd0530/cakezerotest.git
# or add remote to an existing repo:
git remote add origin https://github.com/mjd0530/cakezerotest.git
git push -u origin main
```

---

## License

Add your license file and notice here (e.g. MIT, proprietary).
