# CLAUDE.md — Ema Prović Personal Photography Website

## Project Overview

A static personal website for Ema Prović, a film photographer based in Brussels/London. The site is deployed via GitHub Pages at **www.emaprovic.com** (configured via `CNAME`). There is no build step, bundler, or framework — everything is plain HTML and CSS.

## Repository Structure

```
/
├── index.html        # Home page (hero, about, featured projects)
├── projects.html     # Full projects listing
├── gallery.html      # Photo gallery (masonry grid)
├── style.css         # Single shared stylesheet for all pages
├── images/           # Image assets
│   ├── hero.jpg               # Hero portrait (referenced but not yet committed)
│   ├── the_cycling_series.jpeg
│   └── languishing.jpeg
├── CNAME             # GitHub Pages custom domain → www.emaprovic.com
└── .gitignore        # Ignores "screenshots for inspiration/" and .DS_Store
```

## Architecture Decisions

- **No build system.** No npm, no bundler, no preprocessor. Edit files directly and push.
- **Single CSS file.** `style.css` is shared by all three HTML pages via `<link rel="stylesheet" href="style.css">`.
- **No JavaScript.** The site is intentionally JS-free; keep it that way unless there is a compelling reason.
- **No external CSS frameworks.** All styling is hand-written; do not introduce Bootstrap, Tailwind, etc.
- **Self-contained pages.** Each HTML file includes the full nav and footer; there is no templating system or server-side includes.

## CSS Conventions

### Design Tokens (CSS Custom Properties)

All visual constants are defined as custom properties on `:root` in `style.css`. Always use these tokens; do not hardcode raw values.

| Token | Value | Purpose |
|---|---|---|
| `--font-heading` | `'Courier New', Courier, monospace` | Headings, nav, labels |
| `--font-body` | System sans-serif stack | Body text |
| `--color-bg` | `#fafafa` | Page background |
| `--color-heading` | `#888` | Headings and muted UI text |
| `--color-text` | `#333` | Body text |
| `--color-footer-bg` | `#f0f0f0` | Footer background |
| `--color-placeholder` | `#e0e0e0` | Placeholder photo backgrounds |
| `--color-placeholder-text` | `#aaa` | Placeholder photo labels |
| `--max-width` | `1200px` | Container max-width |
| `--space-xs` | `0.5rem` | Extra-small spacing |
| `--space-sm` | `1rem` | Small spacing |
| `--space-md` | `2rem` | Medium spacing |
| `--space-lg` | `4rem` | Large spacing |
| `--space-xl` | `6rem` | Extra-large spacing |

### Responsive Breakpoints

- **Tablet** (`≤768px`): Single-column hero and project cards; 2-column gallery; single-column footer.
- **Mobile** (`≤480px`): Spacing tokens are reduced; vertical nav stacking; single-column gallery.

### Key CSS Classes

| Class | Description |
|---|---|
| `.container` | Centered wrapper, max-width `1200px`, horizontal padding `--space-md` |
| `.hero` | Two-column grid (photo left, text right) |
| `.about` | Full-width text block, max-width `700px` for prose |
| `.projects-list` | Flex column of `.project-card` items, `8rem` gap between cards |
| `.project-card` | Two-column grid: `3fr` image, `2fr` text |
| `.btn-read` | Pill-shaped outline button for project links |
| `.gallery-grid` | CSS `columns: 3` masonry layout |
| `.placeholder-photo` | Grey placeholder block; use aspect-ratio modifier classes |
| `.page-header` | Top-of-page `<h1>` section on interior pages |

#### Placeholder aspect ratio modifiers (used in `gallery.html`)

`.portrait` (2:3) · `.landscape` (3:2) · `.square` (1:1) · `.wide` (16:9) · `.hero-photo` (4:5)

## Page Layouts

### `index.html` — Home

Sections in order:
1. `<nav>` — logo left, links right
2. `.hero` — photo + name/subtitle
3. `.about` — short bio paragraph
4. `.projects-list` — featured project cards (same markup as `projects.html`)
5. `<footer>` — three columns: name, location, contact email

### `projects.html` — Projects

Sections in order:
1. `<nav>`
2. `.page-header` — `<h1>Projects</h1>`
3. `.projects-list` — all project cards
4. `<footer>`

Each project card (`<article class="project-card">`) contains:
- `.project-image` with an `<img>`
- `.project-text` with `<h3>`, `<p>`, and `.btn-read` anchor

### `gallery.html` — Gallery

Sections in order:
1. `<nav>`
2. `.page-header` — `<h1>Gallery</h1>`
3. `.gallery-grid` — masonry grid of photos/placeholders
4. `<footer>`

## How to Make Common Changes

### Add a new project

1. Add the project image to `images/`.
2. In **both** `projects.html` and `index.html` (featured section), copy an existing `<article class="project-card">` block and update the `src`, `alt`, `<h3>`, `<p>`, and `href`.

### Replace a gallery placeholder with a real photo

In `gallery.html`, swap the `<div>` placeholder for a real `<img>`:

```html
<!-- Before -->
<div class="placeholder-photo portrait">
  <span>Photo 1</span>
</div>

<!-- After -->
<img src="images/your-photo.jpg" alt="Description of the photo">
```

### Add a photo to the gallery

Copy any `<div class="placeholder-photo ...">` block and paste it inside `.gallery-grid`. Choose the aspect-ratio class that matches your photo's orientation.

### Replace the hero photo

Update `<img src="images/hero.jpg" alt="Ema Prović with camera">` in `index.html` and add the image file to `images/`.

### Add a new page

1. Create a new `.html` file.
2. Copy the `<nav>` and `<footer>` blocks verbatim from an existing page.
3. Link `style.css` in `<head>`.
4. Add a link to the new page in the `<nav>` on **all** existing pages.

### Update contact/location info

The footer is duplicated in all three HTML files. Update all three when changing the email address or location.

## Navigation Links

The nav bar contains:
- **Ema Prović** (logo) → `index.html`
- **Projects** → `projects.html`
- **Gallery** → `gallery.html`
- **Substack** → `https://emaprovic.substack.com/` (opens in new tab)

All "Read" buttons on project cards also link to Substack.

## Deployment

The site is hosted on **GitHub Pages**. Pushing to the `master` branch deploys automatically. The `CNAME` file maps the custom domain `www.emaprovic.com`.

There are no CI/CD pipelines, linters, or test suites. Verify changes by opening the HTML files locally in a browser before pushing.

## Coding Conventions

- **Indentation:** 2 spaces in HTML and CSS.
- **HTML attributes:** double quotes.
- **External links:** always include `target="_blank" rel="noopener"`.
- **Images:** always include a descriptive `alt` attribute.
- **Comments:** use HTML `<!-- ... -->` comments to mark sections and guide content editors (see `gallery.html` and `projects.html` for examples).
- **Do not add JavaScript** unless strictly necessary — the site's simplicity is intentional.
- **Do not introduce a build step** — keep it deployable by pushing raw files.
