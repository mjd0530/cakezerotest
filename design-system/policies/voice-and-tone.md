# Voice and tone — AI agent reference

**Audience:** AI agents and engineers generating or reviewing **user-visible UI copy** (labels, buttons, headings, errors, hints, empty states, toasts, `aria-label` / `aria-live` text, agent status messages).

**Scope:** Language, grammar, and formatting for strings in interfaces built from this design system. Visual typography (font tokens) remains in `typography.md` and `typography.json`.

This document synthesizes **Language & Grammar** guidance from the **Lenovo Cake Design System** for use in codegen and design-system-aligned products.

---

## 1. Tone of voice

The brand positions as **tech optimists**. Voice rests on three pillars:

| Pillar | Meaning for UI copy |
|--------|---------------------|
| **Purposeful** | Every string has a clear intent: what the user learns, decides, or does next. |
| **Unexpected** | Prefer direct, human phrasing over generic corporate filler (“successfully completed,” “please note that”). |
| **Brave** | Confident and specific; avoid hedging unless uncertainty is real (e.g. “may take a few minutes”). |

---

## 2. Content principles

Support **clear, inclusive product copy** with these five standards:

| Principle | Guideline |
|-----------|-----------|
| **Accessible** | Aim for roughly **7th-grade reading level** where possible. Pair visible text with correct **accessible names** and descriptions per `accessibility.md` (e.g. `aria-label`, `aria-describedby`, live regions). |
| **Purposeful** | State what the user can do or what happened; tie copy to the **next action** (e.g. “Fix email format” not only “Invalid”). |
| **Concise** | Prefer line lengths **under ~50 characters** where layout allows; keep blocks **under about four lines** before splitting or using progressive disclosure. **Buttons:** **3 words or fewer** when possible (e.g. “Save changes,” “Delete file”). |
| **Conversational** | Use plain, familiar words; **step-by-step** instructions in logical order. |
| **Clear** | **Consistent terminology** across the product (same concept → same label). **Errors** must include **what went wrong** and **how to recover** (retry, edit field, contact support). |

---

## 3. Grammar and formatting

### 3.1 Capitalization

- **Sentence case** is the standard for UI: titles, headings, menu items, list items, buttons, tabs, and table headers.
- **Do:** `Check for updates`
- **Don’t:** `Check for Updates` (title case) for routine UI chrome.

**Exceptions:** Proper nouns, product names, and acronyms as defined by brand (e.g. “USB,” “Wi‑Fi”).

### 3.2 Punctuation

- **Short labels and headings:** No terminal punctuation unless they are full sentences or questions.
- **Full sentences** (body copy, descriptions, errors): Use correct ending punctuation (`.`, `?`, `!` as appropriate).
- **Exclamation points:** Use **sparingly**—for genuine delight or emphasis, not every success message.

### 3.3 Body text and layout

- Prefer **plain language** over jargon; explain technical terms when they must appear.
- Use **layout and visuals** to support complex ideas (don’t rely on long paragraphs in dense modals).
- Avoid **orphans** (single word on its own line) and **widows** (very short last lines) where you control line breaks; in responsive UI, favor shorter clauses and natural wrap.

---

## 4. UI patterns (checklist for agents)

| Context | Guidance |
|---------|----------|
| **Primary button** | Verb + object if needed; ≤3 words when possible. |
| **Secondary / cancel** | “Cancel,” “Back,” “Skip” — single clear word if enough. |
| **Destructive** | Name the action and object (“Delete file”); confirmation body explains irreversibility per `destructive-action` intent. |
| **Field labels** | Noun or short noun phrase; sentence case. Required: mark in label or helper, not placeholder alone. |
| **Placeholder** | Hint or example only; never the only label. |
| **Errors** | Specific, actionable; include recovery. Works with `aria-invalid` and `aria-describedby` per `input.md` / `form-submission` intent. |
| **Success** | Short confirmation; avoid “Success!” every time—vary or use status + next step. |
| **Empty states** | What’s empty + what to do (e.g. “No devices yet — Add a device”). |
| **Agentic / streaming status** | Use `accessibility.md` live-region guidance: short status updates (“Searching…”, “Found 3 results”), not walls of text. |

---

## 5. Relationship to other policies

- **`accessibility.md`** — Screen reader strings, politeness of live regions, focus and announcements.
- **`typography.md`** — Type roles and scales (not wording).
- **Intents** (`form-submission.md`, `outcome-feedback.md`, etc.) — Flow-level copy expectations.

---

## 6. Additional resources

- Lenovo brand tone of voice (broader marketing and brand context): [Lenovo Brand World — Tone of voice](https://brandworld.lenovo.com/tone-of-voice/)

---

*Policy version: 1.0 — Agent-readable voice and tone for UI copy.*
