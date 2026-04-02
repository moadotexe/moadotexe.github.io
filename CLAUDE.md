# CLAUDE.md

## Repository Overview

**moadotexe.github.io** is a personal developer portfolio site deployed via GitHub Pages. It is a **zero-dependency, single-file static site** — the entire application lives in `index.html`.

## File Structure

```
moadotexe.github.io/
├── index.html     # Entire application (HTML + CSS + JS, ~800 lines)
├── README.md      # Minimal readme
└── CLAUDE.md      # This file
```

There is no build step, no package manager, and no tooling configuration.

## Architecture

### Single-file SPA

`index.html` is structured in three major sections:

| Lines | Section | Description |
|-------|---------|-------------|
| 1–9 | HTML head | Meta tags, favicon (inline SVG), description |
| 10–458 | `<style>` | All CSS: design tokens, reset, components, responsive |
| 459–717 | `<body>` | Nav, `#page` mount point, 4 `<template>` elements |
| 719–796 | `<script>` | Client-side router and form handler |

### Client-side Routing

Four pages are defined as `<template id="tpl-{page}">` elements and swapped into `<div id="page">` on navigation:

```
#home      → tpl-home
#projects  → tpl-projects
#about     → tpl-about
#contact   → tpl-contact
```

The router (`navigate()`) performs a 180ms fade-out, clones the template into `#page`, then re-binds `[data-page]` click handlers. History is managed via `history.pushState` and `popstate`.

## Design System

### CSS Custom Properties

Defined in `:root`, used throughout — never hardcode colors:

```css
--bg:       #080c14   /* Page background */
--bg2:      #0e1525   /* Slightly lighter bg */
--surface:  #131b2e   /* Card/panel surfaces */
--border:   rgba(122,162,247,.13)
--text:     #dde4f5   /* Body text */
--muted:    #7888aa   /* Secondary text */
--accent:   #7aa2f7   /* Primary blue accent */
--cyan:     #22d3ee
--green:    #4ade80
--red:      #f87171
--glow:     rgba(122,162,247,.18)
```

### Layout

- `.shell` — centered container, `max-width: 900px`, `padding: 0 1.25rem`
- Responsive breakpoints: `520px` (mobile), `700px` (tablet)
- Use CSS Grid with `auto-fill` / `minmax()` for card grids

### Typography

- Font: `'Courier New', 'Menlo', monospace` — applied globally on `body`
- Never introduce a different font family; the monospace aesthetic is intentional

### Component Conventions

- Section labels use `.section-label` with a `::before` that injects `// `
- Cards follow `.proj-card` pattern: icon, title, description, tags, metadata row
- Tags/badges use `.tag` class
- Buttons: `.btn` base + `.btn-primary` or `.btn-ghost` modifiers
- Utility spacing: `.pt` (padding-top), `.pb` (padding-bottom)

### Visual Effects

These are intentional and should be preserved:
- **Grain overlay** — `body::before` with SVG turbulence filter
- **Scan lines** — `body::after` with repeating-linear-gradient
- **Blinking cursor** on `.brand` via `.cursor` + `@keyframes blink`
- **Glassmorphism nav** — `backdrop-filter: blur(12px)` + semi-transparent bg

## Development Workflow

### Running Locally

No build step required. Open `index.html` directly in a browser, or serve with any static server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

### Deployment

Pushing to `main` deploys automatically via GitHub Pages. No CI/CD pipeline exists.

### Branch Strategy

- `main` — production branch, maps directly to the live site
- Feature/fix work should be done on a separate branch and merged via PR

## Editing Guidelines for AI Assistants

### What to do

- **Keep everything in `index.html`** — do not split into separate CSS/JS files unless explicitly asked
- **Use existing CSS variables** for all colors and spacing
- **Follow BEM-like naming**: `.component-element` pattern
- **Preserve visual effects** (grain, scan lines, cursor blink, transitions)
- **Maintain the monospace aesthetic** — the terminal/dev theme is core to the design
- **Keep JS vanilla** — no frameworks, no npm dependencies
- **Add new pages** by adding a `<template id="tpl-{page}">` and registering the page name in the `PAGES` array

### What NOT to do

- Do not introduce npm, bundlers, or build tools unless explicitly requested
- Do not add external CSS frameworks (Tailwind, Bootstrap, etc.)
- Do not change the color scheme without being asked
- Do not add a web font — the monospace stack is intentional
- Do not modify the router logic unless fixing a specific bug
- Do not add inline styles — use CSS classes instead

### Contact Form

The form submission handler (`handleFormSubmit`) is a **stub** — it only updates the button state visually. To wire it up, replace the stub with a fetch call to a backend service (Formspree, Web3Forms, etc.). Do not implement a real backend without being asked.

## Key Identifiers

| ID / Class | Purpose |
|---|---|
| `#page` | SPA mount point where template content is rendered |
| `#nav-toggle` | Mobile hamburger button |
| `#nav-links` | Nav link list (toggled `.open` on mobile) |
| `#year` | Injected with `new Date().getFullYear()` on init |
| `tpl-home/projects/about/contact` | Page template elements |
| `.shell` | Max-width centered container |
| `.nav`, `.nav-inner` | Sticky top navigation |
| `.brand` | Logo/name in nav |
| `.section-label` | Section heading with `//` prefix |
| `.proj-card` | Project card component |
| `.tag` | Skill/tech badge |
| `.btn`, `.btn-primary`, `.btn-ghost` | Button variants |
| `.leaving` | Class added to `#page` during page exit animation |
