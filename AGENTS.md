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
├── css/
│   └── style.css     # One shared stylesheet – dark theme, CSS variables, responsive
├── js/
│   └── main.js       # One shared script – nav highlight, hamburger, form submit
├── AGENTS.md         # ← you are here
└── README.md         # Developer guide (local setup, deployment, design tokens)
```

---

## Architecture decisions

| Decision | Rationale |
|----------|-----------|
| No framework or build tool | GitHub Pages serves only static files; zero configuration deployment |
| Single CSS file | All pages share one theme; CSS custom properties avoid duplication |
| Single JS file | Loaded by every page; guards with `if (element)` checks make it safe on any page |
| Formspree for the contact form | Provides a real email backend without a server; free tier is sufficient |
| Inline SVG icons | No icon font dependency; icons stay crisp at any resolution |
| No external fonts | System font stack (`'Segoe UI', system-ui, …`) avoids a network round-trip |

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
| `--primary` | `#0d6efd` | CTA buttons, interactive links |
| `--accent` | `#00d4ff` | Cyan highlights, dots, labels |
| `--dark` | `#1a1a2e` | Page background |
| `--darker` | `#0f0f1a` | Navbar, alternate sections |
| `--card-bg` | `#16213e` | Card backgrounds |
| `--white` | `#ffffff` | H1/H2 headings |
| `--text` | `#e0e0e0` | Body text |
| `--text-muted` | `#9e9e9e` | Secondary / helper text |
| `--border` | `rgba(255,255,255,0.08)` | Subtle dividers and card borders |
| `--radius` | `12px` | Cards, buttons |
| `--shadow` | `0 4px 24px rgba(0,0,0,0.4)` | Hover / lifted shadows |
| `--transition` | `0.3s ease` | All `transition:` declarations |

### Responsive breakpoints

The stylesheet has two breakpoints. Add media query overrides at the bottom of the
`/* ===== Responsive ===== */` section in `css/style.css`:

```css
@media (max-width: 768px) { /* tablet / mobile */ }
@media (max-width: 480px) { /* small mobile */ }
```

### CSS structure (section order)

```
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
12. Footer
13. Responsive
```

Add new component styles in a new labelled section **before** the Responsive section.

---

## HTML conventions

### Shared boilerplate per page

Every page follows this skeleton exactly. Copy it when creating a new page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Page Title – Thomas Schulze IT Solutions</title>
  <meta name="description" content="Page-specific description." />
  <link rel="stylesheet" href="css/style.css" />
</head>
<body>

  <nav class="navbar">
    <a href="index.html" class="navbar-brand">Thomas<span>.</span>dev</a>
    <ul class="nav-links">
      <li><a href="index.html">Home</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="projects.html">Projects</a></li>
      <li><a href="contact.html">Contact</a></li>
    </ul>
    <button class="hamburger" aria-label="Toggle navigation">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <main class="page">
    <!-- page content here -->
  </main>

  <footer>
    <p>&copy; 2024 Thomas Schulze IT Solutions &middot; <a href="contact.html">Contact</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

### Navigation – keep all four files in sync

There is **no templating**. When you add or rename a navigation item you must update the
`<ul class="nav-links">` block identically in **all four** HTML files:
`index.html`, `about.html`, `projects.html`, `contact.html`.

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

`js/main.js` is plain vanilla JavaScript targeting **modern evergreen browsers** (ES6+).
It uses `const`, `fetch`, `FormData`, and arrow-free function expressions. No transpiler,
no bundler, and no module system – the file is loaded directly via a `<script>` tag.

### Guard every DOM query

The script is loaded on every page. Use `if (element)` guards before calling methods:

```js
// ✅ correct – safe on pages without the element
const contactForm = document.getElementById('contactForm');
if (contactForm) {
  contactForm.addEventListener('submit', …);
}

// ❌ wrong – throws on pages without the element
document.getElementById('contactForm').addEventListener('submit', …);
```

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
| Add / edit a project card | `projects.html` |
| Update contact email / social links | `about.html`, `contact.html` |
| Change colours or typography | `css/style.css` – `:root` tokens |
| Add a new UI component | `css/style.css` (new labelled section), then use in the relevant HTML |
| Change form behaviour | `js/main.js` – contact form handler |
| Add a new page | New `.html` file + add nav link in all four existing HTML files |
| Change Formspree delivery email | Formspree dashboard (no code change) |
| Replace Formspree | Update `action` on `<form>` and `fetch()` in `js/main.js` |
| Update hero headline or chips | `index.html` |

---

## Do's and Don'ts

### ✅ Do

- Run `python3 -m http.server 3000` and open `http://localhost:3000` to preview changes.
- Use CSS custom property tokens for all colour and spacing values.
- Keep the `<nav>`, `<footer>`, and `<script>` tags identical across all HTML files.
- Validate HTML after edits: `python3 -c "from html.parser import HTMLParser; …"` or any online validator.
- Add `rel="noopener"` to every `target="_blank"` link (security best practice).
- Keep `js/main.js` ES6+ (modern browsers). Use `const`/`let`, `fetch`, and `FormData` freely. Do **not** add a transpiler or bundler.
- Document significant architectural changes in this file (`AGENTS.md`).

### ❌ Don't

- **Don't introduce a framework, bundler, or npm dependency.** The site has zero build steps by design.
- **Don't add a separate stylesheet per page.** One shared `css/style.css` keeps the theme consistent.
- **Don't hard-code colour hex values in HTML or CSS** – use the custom properties.
- **Don't use `document.write()`** or other intrusive DOM patterns.
- **Don't commit `node_modules/`, `.DS_Store`, or editor config files** (add them to `.gitignore` if needed).
- **Don't break the Formspree AJAX pattern** by removing `e.preventDefault()` from the form handler.

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
