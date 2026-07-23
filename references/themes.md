# Themes & Fonts

The template's visual style is driven by CSS variables for colors and fonts. These styles are configured via small CSS snippets in the `resources/themes/` directory.

## Beautiful.ai-inspired Themes

The Beautiful template (`resources/template-beautiful.html`) uses the CSS variables defined in `resources/themes/theme-beautiful.css`. Apply a preset by replacing `{{THEME}}` with one of the following body classes:

- `theme-indigo` — default modern indigo-blue
- `theme-teal` — fresh teal gradient
- `theme-rose` — energetic rose-to-amber
- `theme-amber` — warm amber-to-orange
- `theme-deep` — dark premium mode
- `theme-warm` — warm cream with serif display

The preset class is applied to `<body>`, e.g. `<body class="theme-indigo">`.

## Cyberpunk / Tech / Light / Emerald Themes

The Cyberpunk template (`resources/template.html`) uses font presets in `resources/themes/`:

- `font-modern.css` — standard sans (default)
- `font-serif.css` — serif display titles
- `font-tech.css` — monospace titles
- `font-rounded.css` — rounded titles

Paste the contents of the chosen font preset into the template's `<style>` block if requested by the user.

## Swiss International Style

The Swiss template (`resources/template-swiss.html`) has its own built-in design system. Do not apply the local theme/font snippets to it unless explicitly requested.
