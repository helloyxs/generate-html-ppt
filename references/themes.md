# Themes & Fonts

The template's visual style is driven by CSS variables for colors (e.g., `--cover-bg`) and fonts (e.g., `--font-body`). These styles are configured via small CSS snippets in the `resources/themes/` directory. To apply a style, paste the contents of the chosen `.css` file into the `<style>` block right after the template's `:root { ... }` rule.

**Style Configuration:** 
- **Cyberpunk Dark / Tech Blue / Light / Emerald**: Uses `resources/template.html`. You can apply different font presets by pasting `font-*.css` contents into the `<style>` block.
- **Swiss International Style**: Uses the independent remote template. Do not apply the local theme/font snippets to it unless explicitly requested, as it has its own built-in design system.

**Font Presets (`font-*.css`):**
- `font-modern.css` — Standard sans (default).
- `font-serif.css` — Serif display titles.
- `font-tech.css` — Monospace titles.
- `font-rounded.css` — Rounded titles.

Apply these by pasting into the template's `<style>` block if requested by the user.
