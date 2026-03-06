# thomas-schulze-it-solutions.contact.io

Personal portfolio and contact site for **Thomas Schulze – IT Solutions**, published as a [GitHub Pages](https://pages.github.com/) static site.

**Live URL:** `https://thoooschuuu.github.io/thomas-schulze-it-solutions.contact.io/`

---

## Table of Contents

- [Project overview](#project-overview)
- [Repository structure](#repository-structure)
- [Local development](#local-development)
- [Making changes](#making-changes)
  - [Page content](#page-content)
  - [Styles](#styles)
  - [JavaScript](#javascript)
  - [Contact form (Formspree)](#contact-form-formspree)
- [Deployment](#deployment)
- [Design tokens reference](#design-tokens-reference)

---

## Project overview

A zero-dependency, framework-free static website consisting of four pages:

| Page | File | Purpose |
|------|------|---------|
| Landing | `index.html` | Hero section, value-proposition cards, tech-stack chips |
| About | `about.html` | Bio, skills grid, experience stats, service list |
| Projects | `projects.html` | Project cards with tags and GitHub links |
| Contact | `contact.html` | Contact info + Formspree-backed contact form |

All pages share a single stylesheet (`css/style.css`) and a single script (`js/main.js`).

---

## Repository structure

```
thomas-schulze-it-solutions.contact.io/
├── index.html        # Landing / hero page
├── about.html        # About me page
├── projects.html     # Projects / portfolio page
├── contact.html      # Contact info + form
├── css/
│   └── style.css     # Single shared stylesheet (dark theme, responsive)
├── js/
│   └── main.js       # Vanilla JS: nav, hamburger menu, contact form
├── AGENTS.md         # Documentation for AI coding agents
└── README.md         # This file
```

---

## Local development

### Prerequisites

All you need is a way to serve static files locally. Any of the following work:

- **Python 3** (usually pre-installed on macOS/Linux)
- **Node.js** `npx serve`
- **VS Code** with the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension

### Option 1 – Python (recommended, no install needed)

```bash
# From the repository root
python3 -m http.server 3000
```

Open <http://localhost:3000> in your browser.

### Option 2 – Node.js / npx

```bash
npx serve .
```

Open the URL shown in the terminal (usually <http://localhost:3000>).

### Option 3 – VS Code Live Server

1. Install the **Live Server** extension.
2. Right-click `index.html` → **Open with Live Server**.
3. The browser opens and automatically reloads on every save.

> **Why not just open the HTML file directly?**
> Opening `file://` URLs in a browser works for basic inspection, but the Formspree AJAX call on the contact page and some browsers' security policies work better behind a proper HTTP server.

---

## Making changes

### Page content

Each page is a self-contained HTML file. Navigation, `<head>`, and `<footer>` must be kept in sync across all four files manually – there is no templating engine.

**Checklist when adding or renaming a page:**

1. Add an `<li><a href="newpage.html">Label</a></li>` entry to the `<ul class="nav-links">` block in **all four** existing HTML files.
2. Create the new HTML file, copying the `<nav>` and `<footer>` blocks verbatim from an existing page.
3. Include `<link rel="stylesheet" href="css/style.css" />` and `<script src="js/main.js"></script>`.

### Styles

All CSS lives in `css/style.css`. It is structured in clearly labelled sections:

```
/* ===== CSS Reset & Base ===== */
/* ===== Navigation ===== */
/* ===== Hero / Landing ===== */
/* ===== Buttons ===== */
/* ===== About Page ===== */
/* ===== Projects Page ===== */
/* ===== Contact Page ===== */
/* ===== Footer ===== */
/* ===== Responsive ===== */
```

- Use the CSS custom properties defined in `:root` instead of hard-coded colours. See [Design tokens reference](#design-tokens-reference) below.
- The responsive breakpoints are `768px` and `480px` – add new media query rules at the bottom of the `/* ===== Responsive ===== */` section.

### JavaScript

`js/main.js` does three things:

1. **Active nav link** – marks the `<a>` that matches the current page filename with the class `active`.
2. **Hamburger menu** – toggles `.open` on `.nav-links` and animates the three `<span>` bars into an ✕.
3. **Contact form** – intercepts `submit`, POSTs to Formspree via `fetch()`, and shows either a success message (`#formSuccess`) or an error on the submit button.

The file uses ES6+ syntax (`const`, `fetch`, `FormData`) and targets modern evergreen browsers.
No transpiler or bundler is used – the script is loaded directly via a `<script>` tag.

### Contact form (Formspree)

The form in `contact.html` posts to the Formspree endpoint:

```
https://formspree.io/f/xpwzqgpn
```

- Submissions are delivered to the email address configured in the Formspree dashboard.
- The `_replyto` field maps to the sender's email so replies work from your inbox.
- The hidden `_gotcha` field is a honeypot to discard bot submissions automatically.
- To change the delivery address: log in at [formspree.io](https://formspree.io), navigate to the form, and update the email there. **No code change is needed.**
- To replace Formspree with a different service, update the `action` attribute on the `<form>` element and adjust the `fetch()` call in `js/main.js` accordingly.

---

## Deployment

Deployment is fully automatic via **GitHub Pages**:

1. Push changes to the branch configured as the GitHub Pages source (typically `main`).
2. GitHub Pages rebuilds and publishes the site within ~1 minute.
3. The live URL is `https://thoooschuuu.github.io/thomas-schulze-it-solutions.contact.io/`.

There is no build step, no CI pipeline, and no compilation required.

---

## Design tokens reference

All colours, spacing, and timing values are defined as CSS custom properties on `:root` in `css/style.css`:

| Token | Value | Usage |
|-------|-------|-------|
| `--primary` | `#0d6efd` | Button backgrounds, links |
| `--primary-dark` | `#0a58ca` | Button hover states |
| `--accent` | `#00d4ff` | Cyan highlight, skill dots, section labels |
| `--dark` | `#1a1a2e` | Page background |
| `--darker` | `#0f0f1a` | Navbar, alternate section background |
| `--card-bg` | `#16213e` | Card backgrounds |
| `--white` | `#ffffff` | Headings, important text |
| `--text` | `#e0e0e0` | Body text |
| `--text-muted` | `#9e9e9e` | Secondary / helper text |
| `--border` | `rgba(255,255,255,0.08)` | Subtle borders |
| `--radius` | `12px` | Border radius for cards and buttons |
| `--shadow` | `0 4px 24px rgba(0,0,0,0.4)` | Card / hover shadows |
| `--transition` | `0.3s ease` | All CSS transitions |
| `--font` | `'Segoe UI', system-ui, …` | Font stack |
