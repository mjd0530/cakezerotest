# AI-Readable Forms Page Template

> **AI INSTRUCTIONS**
> This document is a structured template for populating and understanding web forms.
> When filling out any form section below:
> - Read the `[AI_CONTEXT]` block for each form before responding.
> - Fill only the fields marked `[REQUIRED]` at minimum; fill `[OPTIONAL]` fields when data is available.
> - Output values in the format specified by `[FORMAT]`. If no format is given, use plain text.
> - Do not invent data. If a value is unknown, output `null` or leave blank as instructed.
> - Validate your output against any `[RULE]` constraints before finalizing.
> - When a field has `[ENUM]`, only use one of the listed allowed values.
> - Respect `[MAX_LENGTH]` limits strictly.

---

## FORM TYPE: Contact / Inquiry

```
[AI_CONTEXT]
Purpose: Capture a user's contact details and reason for reaching out.
Tone: Professional and concise.
On completion: Submit to contact handler or route to appropriate department.
```

| Field ID         | Label              | Required | Format              | Rules & Notes                                      |
|------------------|--------------------|----------|---------------------|----------------------------------------------------|
| `contact.name`   | Full Name          | REQUIRED | Text                | [MAX_LENGTH: 100] First and last name              |
| `contact.email`  | Email Address      | REQUIRED | email               | [RULE] Must match pattern: `*@*.*`                 |
| `contact.phone`  | Phone Number       | OPTIONAL | E.164 or local      | Example: +1 555-000-1234                           |
| `contact.subject`| Subject            | REQUIRED | Text                | [MAX_LENGTH: 150] Brief summary of inquiry         |
| `contact.message`| Message            | REQUIRED | Multi-line text     | [MAX_LENGTH: 1000] Full inquiry details            |
| `contact.dept`   | Department         | OPTIONAL | [ENUM]              | Allowed: `Sales`, `Support`, `Billing`, `General`  |

---

## FORM TYPE: User Registration / Onboarding

```
[AI_CONTEXT]
Purpose: Create a new user account.
Tone: Friendly, clear, minimal friction.
On completion: POST to /api/register. Expect a confirmation token in response.
```

| Field ID           | Label              | Required | Format              | Rules & Notes                                             |
|--------------------|--------------------|----------|---------------------|-----------------------------------------------------------|
| `reg.first_name`   | First Name         | REQUIRED | Text                | [MAX_LENGTH: 50]                                          |
| `reg.last_name`    | Last Name          | REQUIRED | Text                | [MAX_LENGTH: 50]                                          |
| `reg.email`        | Email Address      | REQUIRED | email               | [RULE] Must be unique in system                           |
| `reg.password`     | Password           | REQUIRED | string (masked)     | [RULE] Min 8 chars, 1 uppercase, 1 number, 1 symbol       |
| `reg.password_confirm` | Confirm Password | REQUIRED | string (masked)   | [RULE] Must match `reg.password` exactly                  |
| `reg.dob`          | Date of Birth      | OPTIONAL | ISO 8601 (YYYY-MM-DD)| [RULE] User must be 13 or older                          |
| `reg.terms`        | Accept Terms       | REQUIRED | boolean             | [RULE] Must be `true` to proceed                         |
| `reg.marketing`    | Marketing Opt-In   | OPTIONAL | boolean             | Default: `false`                                          |

---

## FORM TYPE: Survey / Feedback

```
[AI_CONTEXT]
Purpose: Collect structured feedback or survey responses from a user.
Tone: Neutral and non-leading. Do not suggest answers.
On completion: Store response anonymously unless user opts to identify themselves.
```

| Field ID             | Label                      | Required | Format              | Rules & Notes                                              |
|----------------------|----------------------------|----------|---------------------|------------------------------------------------------------|
| `survey.respondent`  | Name (anonymous option)    | OPTIONAL | Text or "Anonymous" | Default to "Anonymous" if not provided                     |
| `survey.rating`      | Overall Rating             | REQUIRED | Integer             | [RULE] Value between 1 and 5 inclusive                     |
| `survey.category`    | Feedback Category          | REQUIRED | [ENUM]              | Allowed: `Product`, `Support`, `Website`, `Billing`, `Other` |
| `survey.experience`  | Describe Your Experience   | OPTIONAL | Multi-line text     | [MAX_LENGTH: 500]                                          |
| `survey.recommend`   | Would You Recommend Us?    | REQUIRED | [ENUM]              | Allowed: `Yes`, `No`, `Maybe`                              |
| `survey.follow_up`   | OK to Follow Up?           | OPTIONAL | boolean             | If `true`, `contact.email` must also be provided           |

---

## FORM TYPE: File / Document Upload

```
[AI_CONTEXT]
Purpose: Accept one or more file uploads from the user with metadata.
Tone: Instructional. Be explicit about file type and size constraints.
On completion: Upload to storage endpoint and return a file reference ID.
```

| Field ID           | Label              | Required | Format              | Rules & Notes                                              |
|--------------------|--------------------|----------|---------------------|------------------------------------------------------------|
| `upload.label`     | Document Title     | REQUIRED | Text                | [MAX_LENGTH: 100] Human-readable name for the file         |
| `upload.file`      | File               | REQUIRED | Binary / File path  | [RULE] Allowed types: PDF, PNG, JPG, DOCX. [MAX: 10MB]    |
| `upload.category`  | Document Type      | OPTIONAL | [ENUM]              | Allowed: `Invoice`, `ID`, `Contract`, `Photo`, `Other`     |
| `upload.notes`     | Additional Notes   | OPTIONAL | Multi-line text     | [MAX_LENGTH: 300]                                          |

---

## FORM TYPE: Search / Filter

```
[AI_CONTEXT]
Purpose: Allow the user to query or filter a dataset.
Tone: Functional. Only populate fields that narrow the search meaningfully.
On completion: Construct a query string and pass to the search endpoint.
```

| Field ID            | Label              | Required | Format              | Rules & Notes                                              |
|---------------------|--------------------|----------|---------------------|------------------------------------------------------------|
| `search.query`      | Search Keywords    | OPTIONAL | Text                | [MAX_LENGTH: 200] Space-separated keywords                 |
| `search.category`   | Category Filter    | OPTIONAL | [ENUM]              | Use domain-specific enum values                            |
| `search.date_from`  | Date From          | OPTIONAL | ISO 8601 (YYYY-MM-DD)| [RULE] Must be ≤ `search.date_to` if both are set         |
| `search.date_to`    | Date To            | OPTIONAL | ISO 8601 (YYYY-MM-DD)| [RULE] Must be ≥ `search.date_from` if both are set       |
| `search.sort`       | Sort By            | OPTIONAL | [ENUM]              | Allowed: `relevance`, `date_asc`, `date_desc`, `name_asc`  |
| `search.page`       | Page Number        | OPTIONAL | Integer             | Default: `1`. [RULE] Must be ≥ 1                           |
| `search.per_page`   | Results Per Page   | OPTIONAL | Integer             | Default: `20`. Allowed: `10`, `20`, `50`, `100`            |

---

## Global Visual Layout Rules

```
[AI_LAYOUT_RULES]

PAGE BACKGROUND
- Use `semantic.background.base` / `--color-bg-background` (light theme: `primitive.zinc.50`, ~#FAFAFA).
  This applies to the full viewport / page background on ALL forms pages without exception.

FORM CARD
- Every form must be contained within a card component that sits on top of the page background.
- Card background: `semantic.background.surface` / `--color-bg-surface` (white in light theme).
- Card should have visible but subtle elevation: use a soft box-shadow (e.g. 0 1px 4px rgba(0,0,0,0.08))
  or a light border (e.g. 1px solid #E5E7EB).
- Card should have adequate internal padding (recommended: 24px–40px).
- Card should have rounded corners (recommended: border-radius 8px–16px).
- Card width: constrained (recommended max-width 640px for single-column forms), centered on the page background.
- Do NOT render form fields directly onto the grey page background. The card is always required.
```

---

## Global AI Filling Rules

```
[AI_GLOBAL_RULES]

1. REQUIRED fields must always have a non-null, non-empty value.
2. OPTIONAL fields should be populated only if relevant data is available; otherwise output null.
3. [ENUM] fields must use one of the listed values exactly (case-sensitive unless noted).
4. [MAX_LENGTH] is a hard character limit. Truncate gracefully if needed, preserving meaning.
5. [RULE] constraints are validation gates. Violating them should result in an error, not a guess.
6. Boolean fields accept only: true / false (not "yes"/"no" or 1/0 unless otherwise specified).
7. Date fields use ISO 8601 format: YYYY-MM-DD unless a different format is explicitly stated.
8. Email fields must conform to standard email syntax.
9. Do not fabricate data. Use null or an empty string as appropriate when data is unavailable.
10. When in doubt about a field's intent, refer to its [AI_CONTEXT] block before proceeding.
11. Always apply the [AI_LAYOUT_RULES] when generating or rendering any forms page UI.
```

---

*Template version: 1.3 — Gray page (`base`) + white card (`surface`) per `color-semantic.json` light.*