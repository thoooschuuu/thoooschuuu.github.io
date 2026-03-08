# AI Agent Guide – thomas-schulze-it-solutions.contact.io

This file is the authoritative reference for any AI coding agent working on this repository.
Read it fully before making any changes.

---

## What this repository is

A **zero-dependency static GitHub Pages website** for Thomas Schulze, a freelance software
engineer based in Germany. There is no framework, no build system, and no package manager.
Every file you see is exactly what the browser receives.

**Live site:** `https://thoooschuuu.github.io/thomas-schulze-it-solutions.contact.io/`

---

## Repository map

```
thomas-schulze-it-solutions.contact.io/
├── index.html        # Landing / hero page
├── about.html        # About me (bio, skills, stats, services)
├── projects.html     # Project portfolio cards
├── contact.html      # Contact details + Formspree form
├── impressum.html    # Legal notice (Impressum)
├── css/
│   └── style.css     # One shared stylesheet – dark/light theme, CSS variables, responsive
├── fonts/
│   └── arimo-*.woff2 # Self-hosted Arimo font files
├── img/
│   ├── logo.svg      # Full logo
│   └── logo-icon.svg # Icon-only logo (navbar + favicon)
├── js/
│   ├── i18n.js       # DE/EN translations + project data rendering
│   └── main.js       # Theme toggle, nav highlight, hamburger, contact form
├── AGENTS.md         # ← you are here
└── README.md         # Developer guide (local setup, deployment, design tokens)
```

---

## Architecture decisions

| Decision | Rationale |
|----------|-----------|
| No framework or build tool | GitHub Pages serves only static files; zero configuration deployment |
| Single CSS file | All pages share one theme; CSS custom properties avoid duplication |
| Two JS files (`i18n.js` + `main.js`) | `i18n.js` loads first and exposes `window.i18n`; `main.js` relies on it. Guards with `if (element)` checks make them safe on any page |
| Self-hosted Arimo font (`fonts/`) | Avoids a third-party network request; font files committed to the repo |
| Dark / light theme toggle | Stored in `localStorage` (`ts_theme`); detected by an inline `<script>` in each `<head>` before the stylesheet loads, preventing a flash of wrong theme |
| DE / EN language toggle | Handled entirely by `js/i18n.js`; language choice stored in `localStorage` (`ts_lang`) |
| Formspree for the contact form | Provides a real email backend without a server; free tier is sufficient |
| Inline SVG icons | No icon font dependency; icons stay crisp at any resolution |
| SVG logo in `img/` | Scalable logo used in navbar and as favicon |

---

## CSS conventions

### Custom properties (design tokens)

**Always** use the tokens defined in `:root` in `css/style.css` instead of hard-coded values:

```css
/* ✅ correct */
color: var(--accent);
border-radius: var(--radius);
transition: color var(--transition);

/* ❌ wrong */
color: #00d4ff;
border-radius: 12px;
```

| Token | Hex / value | When to use |
|-------|-------------|-------------|
| `--primary` | `#ffc250` | CTA buttons, interactive links (amber/gold) |
| `--primary-dark` | `#e6a832` | Button hover states |
| `--primary-darker` | `#c48a10` | Button active / deep hover states |
| `--secondary` | `#6c757d` | Secondary UI elements |
| `--accent` | `#ffc250` | Highlights, dots, section labels |
| `--accent-light` | `#ffd580` | Lighter accent variant |
| `--dark` | `#1a1710` | Page background (dark mode) |
| `--darker` | `#100e08` | Navbar, alternate sections |
| `--light` | `#f8f9fa` | Page background (light mode) |
| `--card-bg` | `#1e1b10` | Card backgrounds |
| `--white` | `#ffffff` | H1/H2 headings |
| `--text` | `#e0e0e0` | Body text |
| `--text-muted` | `#9e9e9e` | Secondary / helper text |
| `--border` | `rgba(255,194,80,0.12)` | Subtle dividers and card borders |
| `--font` | `'Arimo', 'Segoe UI', system-ui, …` | Font stack |
| `--radius` | `12px` | Cards, buttons |
| `--shadow` | `0 4px 24px rgba(0,0,0,0.5)` | Hover / lifted shadows |
| `--transition` | `0.3s ease` | All `transition:` declarations |
| `--navbar-bg` | `rgba(16,14,8,0.92)` | Navbar background (theme-sensitive) |
| `--dot-color` | `rgba(255,255,255,0.04)` | Background dot grid (dark mode) |
| `--surface-subtle` | `rgba(255,255,255,0.06)` | Subtle surface tint |
| `--surface-faint` | `rgba(255,255,255,0.04)` | Faint surface tint |
| `--outline-border` | `rgba(255,255,255,0.25)` | Outline button border |
| `--outline-border-hover` | `rgba(255,255,255,0.5)` | Outline button border on hover |
| `--outline-bg-hover` | `rgba(255,255,255,0.08)` | Outline button fill on hover |

> **Light mode:** `html[data-theme="light"]` overrides the theme-sensitive tokens. Never hard-code dark-mode colours in component rules – always use the tokens.

### Responsive breakpoints

The stylesheet has two breakpoints. Add media query overrides at the bottom of the
`/* ===== Responsive ===== */` section in `css/style.css`:

```css
@media (max-width: 768px) { /* tablet / mobile */ }
@media (max-width: 480px) { /* small mobile */ }
```

### CSS structure (section order)

```
0.  Arimo Font (locally hosted)
1.  CSS Reset & Base
2.  Navigation
3.  Page Container
4.  Hero / Landing
5.  Buttons
6.  Tech Stack Chips
7.  Section Styles
8.  About Page
9.  Projects Page
10. Contact Page
11. Contact Form
12. Language Toggle
13. Theme Toggle
14. Footer
15. Impressum Page
16. Light Mode
17. Responsive
```

Add new component styles in a new labelled section **before** the Responsive section.

---

## HTML conventions

### Shared boilerplate per page

Every page follows this skeleton exactly. Copy it when creating a new page:

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Page Title – Thomas Schulze IT Solutions</title>
  <meta name="description" content="Page-specific description." />
  <link rel="icon" type="image/svg+xml" href="img/logo-icon.svg" />
  <script>
    (function () {
      var t = localStorage.getItem('ts_theme');
      if (!t) { t = window.matchMedia && window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark'; }
      document.documentElement.setAttribute('data-theme', t);
    })();
  </script>
  <link rel="stylesheet" href="css/style.css" />
</head>
<body>

  <nav class="navbar">
    <a href="index.html" class="navbar-brand"><img src="img/logo-icon.svg" alt="Thomas Schulze IT Solutions" /></a>
    <ul class="nav-links">
      <li><a href="index.html" data-i18n="nav.home">Home</a></li>
      <li><a href="about.html" data-i18n="nav.about">About</a></li>
      <li><a href="projects.html" data-i18n="nav.projects">Projects</a></li>
      <li><a href="contact.html" data-i18n="nav.contact">Contact</a></li>
    </ul>
    <button class="hamburger" aria-label="Toggle navigation">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <main class="page">
    <!-- page content here -->
  </main>

  <footer>
    <p>&copy; 2024 Thomas Schulze IT Solutions &middot; <a href="contact.html" data-i18n="footer.contact">Contact</a> &middot; <a href="impressum.html" data-i18n="footer.impressum">Impressum</a></p>
    <div class="footer-controls">
      <div class="lang-toggle" aria-label="Language switcher">
        <button class="lang-btn" data-lang="de">DE</button>
        <span class="lang-toggle-sep">|</span>
        <button class="lang-btn" data-lang="en">EN</button>
      </div>
      <button class="theme-btn" id="themeToggle" aria-label="Switch colour theme">
        <svg class="theme-icon-sun" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <circle cx="12" cy="12" r="5"/><line x1="12" y1="1" x2="12" y2="3"/><line x1="12" y1="21" x2="12" y2="23"/>
          <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
          <line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/>
          <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
        </svg>
        <svg class="theme-icon-moon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
        </svg>
      </button>
    </div>
  </footer>

  <script src="js/i18n.js"></script>
  <script src="js/main.js"></script>
</body>
</html>
```

### Navigation – keep all five files in sync

There is **no templating**. When you add or rename a navigation item you must update the
`<ul class="nav-links">` block identically in **all five** HTML files:
`index.html`, `about.html`, `projects.html`, `contact.html`, `impressum.html`.
You must also add the corresponding translation key to both `en` and `de` in `js/i18n.js`.

### Section pattern

Content sections use this consistent structure:

```html
<div class="section">                          <!-- max-width: 1100px, centred -->
  <div class="section-header">
    <span class="section-label">Label</span>   <!-- small caps accent label -->
    <h1 class="section-title">Heading</h1>
    <p class="section-desc">Short description.</p>
  </div>
  <!-- grid / card content here -->
</div>
```

### Card pattern (projects page)

```html
<article class="project-card">
  <div class="project-icon"><!-- emoji or SVG --></div>
  <h2 class="project-title">Title</h2>
  <p class="project-desc">Description.</p>
  <div class="project-tags">
    <span class="tag">Tag</span>
  </div>
  <div class="project-links">
    <a href="…" class="project-link" target="_blank" rel="noopener">
      <!-- inline SVG icon --> Link text
    </a>
  </div>
</article>
```

---

## JavaScript conventions

Two scripts are loaded by every page – `js/i18n.js` first, then `js/main.js`. Both are plain vanilla JavaScript targeting **modern evergreen browsers** (ES6+). They use `const`, `fetch`, `FormData`, and function expressions. No transpiler, no bundler, and no module system.

### Guard every DOM query

Both scripts are loaded on every page. Use `if (element)` guards before calling methods:

```js
// ✅ correct – safe on pages without the element
const contactForm = document.getElementById('contactForm');
if (contactForm) {
  contactForm.addEventListener('submit', …);
}

// ❌ wrong – throws on pages without the element
document.getElementById('contactForm').addEventListener('submit', …);
```

### Theme toggle

`main.js` wires the `#themeToggle` button. An inline `<script>` in each page's `<head>` runs before the stylesheet to set `data-theme` on `<html>` from `localStorage` (key `ts_theme`), preventing a flash of the wrong theme. `main.js` only wires the click handler – it reads the attribute already set by that inline snippet.

```js
// Inline snippet in <head> (must stay self-contained):
var t = localStorage.getItem('ts_theme');
if (!t) { t = window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark'; }
document.documentElement.setAttribute('data-theme', t);
```

### i18n (language toggle)

`js/i18n.js` exposes `window.i18n`. It:
- Reads the preferred language from `localStorage` (key `ts_lang`), defaulting to `'de'`.
- Applies translations by reading `data-i18n` (text) and `data-i18n-html` (innerHTML) attributes.
- Dynamically renders the projects grid on `projects.html` from embedded project data.
- Wires the `.lang-btn` buttons in the footer.

When adding user-visible text, always add a `data-i18n="key"` attribute on the element and add the corresponding entry to **both** `en` and `de` in `js/i18n.js`.

### Contact form submission

The form uses the **Formspree AJAX** pattern:

```js
fetch(contactForm.action, {            // action = https://formspree.io/f/xpwzqgpn
  method: 'POST',
  body: new FormData(contactForm),
  headers: { Accept: 'application/json' }
}).then(function (response) {
  if (response.ok) { /* show #formSuccess */ }
  else             { /* update submit button text */ }
});
```

Do **not** remove `e.preventDefault()` – it prevents the default browser form navigation
so the AJAX handler can show inline feedback.

---

## Contact form details

| Attribute | Value |
|-----------|-------|
| Endpoint | `https://formspree.io/f/xpwzqgpn` |
| Method | `POST` (AJAX via `fetch()`) |
| Email field name | `_replyto` (maps to reply-to header) |
| Spam trap | hidden `_gotcha` input (honeypot) |
| Success element | `#formSuccess` (hidden `<div>`, shown on `response.ok`) |

To change the delivery email: update it in the **Formspree dashboard** – no code change needed.

---

## What to change and where

| Goal | File(s) to edit |
|------|----------------|
| Update personal bio or stats | `about.html` |
| Add / edit a project card | `js/i18n.js` (project data array) |
| Update contact email / social links | `about.html`, `contact.html` |
| Change colours or typography | `css/style.css` – `:root` tokens |
| Add a new UI component | `css/style.css` (new labelled section), then use in the relevant HTML |
| Change form behaviour | `js/main.js` – contact form handler |
| Add a new page | New `.html` file + add nav link in all five existing HTML files + add translation keys in `js/i18n.js` |
| Change Formspree delivery email | Formspree dashboard (no code change) |
| Replace Formspree | Update `action` on `<form>` and `fetch()` in `js/main.js` |
| Update hero headline or chips | `index.html` + translation keys in `js/i18n.js` |
| Add or update a translation string | `js/i18n.js` – both `en` and `de` objects |
| Change the theme colour palette | `css/style.css` – `:root` tokens and `html[data-theme="light"]` overrides |
| Replace or update the logo | `img/logo.svg` and/or `img/logo-icon.svg` |
| Update the Impressum | `impressum.html` |

---

## Do's and Don'ts

### ✅ Do

- Run `python3 -m http.server 3000` and open `http://localhost:3000` to preview changes.
- Use CSS custom property tokens for all colour and spacing values.
- Keep the `<nav>`, `<footer>`, and `<script>` tags identical across all HTML files.
- Validate HTML after edits: `python3 -c "from html.parser import HTMLParser; …"` or any online validator.
- Add `rel="noopener"` to every `target="_blank"` link (security best practice).
- Keep `js/main.js` and `js/i18n.js` ES6+ (modern browsers). Use `const`/`let`, `fetch`, and `FormData` freely. Do **not** add a transpiler or bundler.
- Add `data-i18n` / `data-i18n-html` attributes to every user-visible string and provide both `en` and `de` translations in `js/i18n.js`.
- Document significant architectural changes in this file (`AGENTS.md`).

### ❌ Don't

- **Don't introduce a framework, bundler, or npm dependency.** The site has zero build steps by design.
- **Don't add a separate stylesheet per page.** One shared `css/style.css` keeps the theme consistent.
- **Don't hard-code colour hex values in HTML or CSS** – use the custom properties.
- **Don't use `document.write()`** or other intrusive DOM patterns.
- **Don't commit `node_modules/`, `.DS_Store`, or editor config files** (add them to `.gitignore` if needed).
- **Don't break the Formspree AJAX pattern** by removing `e.preventDefault()` from the form handler.
- **Don't add user-visible text without a translation key** in both `en` and `de` in `js/i18n.js`.

---

## Local development quick-start

```bash
# Clone the repository
git clone https://github.com/thoooschuuu/thomas-schulze-it-solutions.contact.io.git
cd thomas-schulze-it-solutions.contact.io

# Serve locally (Python – no install needed)
python3 -m http.server 3000

# Open in browser
open http://localhost:3000        # macOS
xdg-open http://localhost:3000    # Linux
start http://localhost:3000       # Windows
```

See `README.md` for full local development instructions including Node.js and VS Code alternatives.

---

## Deployment

Push to the GitHub Pages source branch (default: `main`). GitHub builds and publishes
automatically within ~1 minute. No CI pipeline, no build step, no configuration needed.
