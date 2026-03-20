# Reference CSS (theme variables)

Static mirrors of semantic tokens as **CSS custom properties**. Use these files in consuming apps (import in global CSS or copy into your build). **JSON under `design-system/tokens/` remains the source of truth** — update these CSS files when tokens change (or generate them in a future build step).

| File | When to load |
|------|----------------|
| `design-tokens.css` | Default theme — align with `color-semantic.json` |
| `design-tokens-lenovo.css` | After `design-tokens.css` — variables apply under `[data-theme="lenovo"]`; align with `color-semantic-lenovo.json` |

Example:

```html
<link rel="stylesheet" href="path/to/design-system/css/design-tokens.css" />
<link rel="stylesheet" href="path/to/design-system/css/design-tokens-lenovo.css" />
```

```html
<html data-theme="lenovo">
```

No Storybook or Node dev server is required to use these files.
