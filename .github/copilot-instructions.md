# GitHub Copilot Instructions — Yellow Craft Showcase

## 1. Project Overview

This is the official showcase website for **Yellow Craft**, a French software-development micro-business (auto-entrepreneur / SASU). The owner is an experienced software engineer offering: custom software development, web development, technical audits, programming consultancy, and programming teaching.

**Primary goals (in order of priority)**

1. **Blazing performance** — Lighthouse score ≥ 98/100 on all four categories (Performance, Accessibility, Best Practices, SEO) measured on a production build served over HTTPS.
2. **SEO excellence** — crawlable, well-structured, rich in metadata, with correct schema.org markup.
3. **Full accessibility** — European Accessibility Act (EAA) compliant, WCAG 2.1 Level AA minimum.
4. **Great UX** — mobile-first, responsive, clean, professional, light/dark mode.
5. **Minimal footprint** — no CSS/JS frameworks; every byte must be justified.
6. **Bilingual** — all user-facing content available in both French 🇫🇷 and English 🇬🇧.

---

## 2. Technology Constraints

| Layer | Allowed | Forbidden |
|---|---|---|
| Markup | Semantic HTML5 only | React, Vue, Angular, Svelte, template engines |
| Styling | Vanilla CSS3 (custom properties, flexbox, grid, `@layer`, `@container`) | Tailwind, Bootstrap, any CSS-in-JS, SASS/LESS (unless trivially replaced) |
| Scripting | Minimal vanilla ES2022+ (type="module") | jQuery, Alpine, HTMX, any JS framework |
| Build tooling | Optional lightweight bundler (Vite, esbuild) for dev convenience | webpack, heavy meta-frameworks (Next, Nuxt, Astro, etc.) |
| Backend | Minimal server (Node/Express, Python/FastAPI, PHP, or serverless function) **only** for the contact form | No CMS, no heavy back-end framework |
| Fonts | Self-hosted `woff2` only, declared with `font-display: swap` | Google Fonts CDN at runtime |
| Icons | Inline SVG or `<symbol>` sprite | Icon-font libraries (FontAwesome, etc.) |

**Rule of thumb:** if a feature can be achieved without JavaScript, do not add JavaScript.

---

## 3. Project Structure

```
yellow-craft-showcase/
├── .github/
│   └── copilot-instructions.md   ← this file
├── public/                        ← static assets (images, fonts, favicon)
│   ├── fonts/
│   ├── images/
│   └── icons/
├── src/
│   ├── css/
│   │   ├── base.css              ← reset, custom properties, typography
│   │   ├── layout.css            ← header, footer, nav, main grid
│   │   ├── components/           ← burger-menu.css, theme-switch.css, form.css …
│   │   └── pages/                ← home.css, contact.css, legal.css …
│   ├── js/
│   │   ├── main.js               ← entry point (type="module")
│   │   ├── theme.js              ← light/dark logic
│   │   ├── i18n.js               ← language switching logic
│   │   └── burger-menu.js        ← mobile nav toggle
│   ├── i18n/
│   │   ├── fr.json               ← French strings
│   │   └── en.json               ← English strings
│   └── partials/                 ← reusable HTML snippets (header, footer…)
├── pages/
│   ├── index.html                ← Home (French default)
│   ├── contact.html
│   ├── mentions-legales.html
│   ├── politique-de-confidentialite.html
│   ├── politique-de-cookies.html
│   ├── accessibilite.html
│   └── plan-du-site.html
├── api/
│   └── contact.js (or .py/.php)  ← serverless/backend contact form handler
├── .gitignore
├── package.json                  ← only if a build step is needed
└── README.md
```

---

## 4. Pages

### 4.1 Home (`index.html`)

Sections (in order):
1. **Hero** — tagline + primary CTA ("Contact me" / "Me contacter")
2. **Services** — card grid: Software Engineering, Web Development, Technical Audits, Consulting, Teaching
3. **About** — short bio, values, tech stack
4. **Testimonials** (optional) — if available
5. **Contact CTA** — link to contact page

### 4.2 Contact (`contact.html`)

- Accessible, semantic `<form>` with proper `<label>` and `<fieldset>`
- Required fields: Name, Email, Subject (dropdown), Message
- Honeypot field (hidden, never shown to users, checked server-side)
- CSRF token generated server-side and validated on submission
- Client-side validation only as UX enhancement; never trust it for security
- Server-side: rate-limiting (e.g., 5 submissions / hour per IP), email sanitisation, no raw HTML in email body
- Success/error feedback injected via ARIA live region (`aria-live="polite"`)
- No third-party form services (Formspree, etc.) — use own back-end endpoint `/api/contact`

### 4.3 Legally mandatory pages (French law)

| Slug | Title (FR) | Title (EN) | Required by |
|---|---|---|---|
| `mentions-legales` | Mentions légales | Legal Notice | LCEN art. 6 |
| `politique-de-confidentialite` | Politique de confidentialité | Privacy Policy | RGPD / GDPR |
| `politique-de-cookies` | Politique de cookies | Cookie Policy | Directive ePrivacy |
| `accessibilite` | Déclaration d'accessibilité | Accessibility Statement | EAA / RGAA |
| `plan-du-site` | Plan du site | Sitemap | Best practice |

#### Mentions légales must include
- Nom de l'éditeur (natural person), adresse, SIRET/SIREN, numéro de TVA intracommunautaire si applicable
- Directeur de la publication
- Hébergeur (nom, adresse, contact)
- Contact : email et/ou téléphone

#### Politique de confidentialité must include
- Responsable du traitement
- Types de données collectées, finalités, base légale
- Durée de conservation
- Droits des utilisateurs (RGPD art. 13-22) and how to exercise them (DPO contact or owner contact)
- Transferts hors UE (if any)
- Cookies and trackers section (or separate page)

#### Déclaration d'accessibilité (RGAA)
- Conformity level (non-conforme / partiellement conforme / conforme)
- Date of last audit
- Known non-compliances with justification
- Contact for reporting accessibility problems
- Recourse options (Défenseur des droits)

---

## 5. HTML Guidelines

- Use the correct semantic element for every purpose: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<figure>`, `<figcaption>`, `<time>`, `<address>`, etc.
- `<h1>` appears **exactly once** per page and describes the page topic (not the logo).
- Heading levels must be strictly hierarchical (no skipping levels).
- All images must have a meaningful `alt` attribute; decorative images use `alt=""` and `role="presentation"`.
- Every interactive element (`<a>`, `<button>`, form controls) must have a **visible focus indicator** — never `outline: none` without a custom replacement.
- Use `<button>` for actions (not styled `<div>` or `<span>`).
- Use `<a href>` only for navigation (not for JS-triggered actions).
- `lang` attribute must be set on `<html>` (`lang="fr"` default, `lang="en"` for the English version).
- Declare the page charset: `<meta charset="UTF-8">`.
- Declare `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Use `<link rel="canonical" href="…">` on every page.

### Language switching
- Detect browser/OS language on first visit; default to French.
- Persist user preference in `localStorage`.
- Switching language updates `document.documentElement.lang`, all `data-i18n` element text nodes, and page `<title>` and `<meta name="description">`.
- Provide `<link rel="alternate" hreflang="fr" href="…">` and `<link rel="alternate" hreflang="en" href="…">` in `<head>` on every page.

---

## 6. CSS Guidelines

### Architecture

- Use **CSS custom properties** (variables) as the single source of truth for colours, spacing, typography, and breakpoints.
- Organise with **`@layer`**: `@layer reset, base, layout, components, utilities, overrides;`
- Use **BEM-inspired naming**: `.block__element--modifier` (kebab-case).
- Never use `!important` except inside a `utilities` layer to intentionally override.
- Avoid inline styles (except dynamically injected values via JS — e.g., CSS custom property overrides on the root).

### Custom properties skeleton

```css
:root {
  /* Colour palette – light mode defaults */
  --color-primary: #f5c400;       /* Yellow Craft brand yellow */
  --color-primary-dark: #c49a00;
  --color-bg: #ffffff;
  --color-bg-alt: #f8f8f8;
  --color-text: #1a1a1a;
  --color-text-muted: #555555;
  --color-border: #dddddd;
  --color-error: #d32f2f;
  --color-success: #2e7d32;
  --color-focus: #005fcc;         /* High-contrast focus ring */

  /* Typography */
  --font-sans: 'InterVariable', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;
  --text-base: 1rem;             /* 16 px baseline */
  --text-sm: 0.875rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 2rem;
  --text-4xl: 2.75rem;
  --leading-body: 1.6;
  --leading-heading: 1.2;

  /* Spacing scale (4-pt grid) */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-24: 6rem;

  /* Layout */
  --container-max: 72rem;        /* 1152 px */
  --container-padding: var(--space-4);
  --nav-height: 4rem;
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;

  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-base: 250ms ease;
}

/* Dark mode — prefer-color-scheme automatic */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #121212;
    --color-bg-alt: #1e1e1e;
    --color-text: #f0f0f0;
    --color-text-muted: #aaaaaa;
    --color-border: #333333;
  }
}

/* Dark mode — manual toggle via data attribute */
[data-theme="dark"] {
  --color-bg: #121212;
  --color-bg-alt: #1e1e1e;
  --color-text: #f0f0f0;
  --color-text-muted: #aaaaaa;
  --color-border: #333333;
}

[data-theme="light"] {
  --color-bg: #ffffff;
  --color-bg-alt: #f8f8f8;
  --color-text: #1a1a1a;
  --color-text-muted: #555555;
  --color-border: #dddddd;
}
```

### Breakpoints (mobile-first)

```css
/* base styles target mobile (< 600 px) */
@media (min-width: 600px)  { /* sm */ }
@media (min-width: 900px)  { /* md */ }
@media (min-width: 1200px) { /* lg */ }
@media (min-width: 1536px) { /* xl */ }
```

### Typography
- Use `clamp()` for fluid heading sizes; never fixed `px` sizes for headings.
- Body text minimum **16 px** (never smaller on mobile).
- Line length: 45–85 characters (`max-width: 75ch` on text containers).
- Minimum contrast ratio 4.5:1 for normal text, 3:1 for large text (WCAG AA).

---

## 7. Accessibility Requirements

- **WCAG 2.1 Level AA** is the minimum target; aim for AAA where practical.
- **RGAA 4.1** (French WCAG transposition) compliance is required for the accessibility statement.
- Skip navigation link: `<a href="#main-content" class="skip-link">Skip to main content</a>` as the very first element in `<body>`.
- All focus states must be visible and have at least 3:1 contrast ratio against adjacent colours.
- `aria-label` or `aria-labelledby` must be provided for every `<nav>` landmark.
- Dialog/modal overlays must trap focus and restore it on close; use `inert` attribute on background content.
- Burger menu button: `aria-expanded`, `aria-controls` pointing to the `<nav>` element id.
- Theme-switch button: `aria-pressed` or `role="switch"` with `aria-checked`.
- Language-switch button/link: `aria-label` indicating the **target** language (e.g., "Switch to English", "Passer en français").
- Form error messages: associate via `aria-describedby` to the relevant `<input>`.
- All icon-only buttons must have either a visually-hidden `<span>` label or `aria-label`.
- Animations: respect `prefers-reduced-motion` — wrap all non-essential transitions/animations.
- Reading order must match visual order (avoid `order` / negative `margin` tricks that break DOM order).

---

## 8. JavaScript Guidelines

- All JS is **progressively enhanced** — the page must be fully usable without JS (except for the language switcher state persistence and theme toggle state persistence).
- Use `type="module"` on all `<script>` tags; no `<script>` without `defer` or `type="module"`.
- Do **not** use `document.write`, `innerHTML` for user-supplied content (XSS risk), or `eval`.
- DOM manipulation must use `textContent` for text nodes and `createElement`/`appendChild` for structure.
- Async operations use the Fetch API with proper error handling and user feedback.
- Never block the main thread; use `requestAnimationFrame` for visual updates.

### Theme toggle (`theme.js`)
```js
// Initialise from localStorage or OS preference
const stored = localStorage.getItem('theme');
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
const theme = stored ?? (prefersDark ? 'dark' : 'light');
document.documentElement.dataset.theme = theme;

// Toggle function
export function toggleTheme() {
  const next = document.documentElement.dataset.theme === 'dark' ? 'light' : 'dark';
  document.documentElement.dataset.theme = next;
  localStorage.setItem('theme', next);
  // update aria-checked / aria-pressed on the toggle button
}

// Also listen for OS preference changes
window.matchMedia('(prefers-color-scheme: dark)')
  .addEventListener('change', (e) => {
    if (!localStorage.getItem('theme')) {
      document.documentElement.dataset.theme = e.matches ? 'dark' : 'light';
    }
  });
```

### Burger menu (`burger-menu.js`)
- Toggle a CSS class (e.g., `.nav--open`) on the `<nav>` element.
- Set `aria-expanded` on the button.
- Lock body scroll when menu is open: `document.body.style.overflow = 'hidden'`.
- Close the menu on: Escape key, click outside the nav, `focusout` from the nav.
- CSS handles the animation (slide-in / fade); JS only manages the class and ARIA state.
- Above the `md` breakpoint, the menu is always visible and the burger button is hidden (`display: none`).

### i18n (`i18n.js`)
- On page load, load `fr.json` or `en.json` based on stored/detected locale.
- All translatable text elements carry a `data-i18n="key"` attribute.
- `updatePageText(locale)` iterates `querySelectorAll('[data-i18n]')` and sets `textContent`.
- Titles, meta descriptions, and `lang` attribute are also updated.
- Persist locale to `localStorage`.

---

## 9. SEO Requirements

- Every page must have a unique, descriptive `<title>` (≤ 60 characters).
- Every page must have a unique `<meta name="description">` (120–160 characters).
- Use Open Graph tags (`og:title`, `og:description`, `og:image`, `og:url`, `og:type`, `og:locale`, `og:locale:alternate`).
- Use Twitter Card tags (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`).
- Use `schema.org` JSON-LD on the home page: `LocalBusiness` or `ProfessionalService` type.
- Use `schema.org` `ContactPage` type on the contact page.
- Provide a `sitemap.xml` at the root (auto-generated or manually maintained).
- Provide a `robots.txt` at the root allowing all crawlers.
- Use descriptive, kebab-case URL slugs (already enforced by the file names above).
- Images: use `<picture>` with `avif`/`webp`/`jpg` sources; always set `width` and `height` to prevent CLS; lazy-load below-the-fold images with `loading="lazy"`.
- Use `rel="preload"` for the hero image and the main font file.
- Critical CSS should be inlined in `<style>` in `<head>` for above-the-fold content.

---

## 10. Performance Requirements

- **Cumulative Layout Shift (CLS):** < 0.1 — always set explicit `width`/`height` on images and embeds.
- **Largest Contentful Paint (LCP):** < 2.5 s — preload hero image, inline critical CSS, self-host fonts.
- **First Input Delay / Interaction to Next Paint (INP):** < 200 ms — keep JS minimal and non-blocking.
- **Total Blocking Time (TBT):** < 200 ms — no heavy synchronous JS.
- HTTP headers (configure on the server/CDN):
  - `Cache-Control: public, max-age=31536000, immutable` for hashed static assets.
  - `Cache-Control: no-cache` for HTML files.
  - `Content-Security-Policy` (see §11).
  - `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`.
  - `X-Content-Type-Options: nosniff`.
  - `X-Frame-Options: DENY`.
  - `Referrer-Policy: strict-origin-when-cross-origin`.
  - `Permissions-Policy: camera=(), microphone=(), geolocation=()`.
- Compress assets: Brotli first, gzip fallback.
- Minify HTML, CSS, and JS in production build.

---

## 11. Security Requirements

### Content Security Policy
Define a strict CSP header (not `<meta>` tag — HTTP header only, to avoid injection):

```
Content-Security-Policy:
  default-src 'none';
  script-src 'self';
  style-src 'self';
  font-src 'self';
  img-src 'self' data:;
  connect-src 'self';
  form-action 'self';
  base-uri 'self';
  frame-ancestors 'none';
```

If any inline script is unavoidable (e.g., the theme initialisation snippet to prevent FOUC), use a **nonce** — never `'unsafe-inline'`.

### Contact form security
- **CSRF protection:** Generate a token server-side (e.g., signed JWT or random token stored in session); include as a hidden field; verify on submission.
- **Honeypot:** Add a hidden `<input name="website" tabindex="-1" autocomplete="off">` field; reject submissions where it is filled.
- **Rate limiting:** Maximum 5 requests per IP per hour; return HTTP 429 with a `Retry-After` header.
- **Input sanitisation:** Strip/escape all HTML on the server before including in the email body; use a library (e.g., `DOMPurify` server-side or `sanitize-html` in Node).
- **Email sending:** Use a transactional email provider (Resend, Mailgun, SendGrid) via their API — never expose SMTP credentials in client code.
- **No logging of personal data** beyond what is necessary for debugging (truncate IPs after rate-limit window expires).

---

## 12. Light/Dark Mode

- Default to OS preference (`prefers-color-scheme`).
- Allow manual override persisted in `localStorage`.
- The theme must be applied **before** the first paint to avoid flash-of-unstyled-content (FOUC). Inject a minimal `<script>` (nonce-protected) in `<head>` **before** any stylesheet link, that reads `localStorage` and sets `document.documentElement.dataset.theme`.
- The theme toggle UI element:
  - Must be keyboard-accessible.
  - Must use `role="switch"` and `aria-checked` (or `aria-pressed` with clear labelling).
  - Must have a visible label (not icon-only without accessible name).
  - Icons: sun/moon SVG inline; no icon fonts.

---

## 13. Internationalisation (i18n)

- Default language: **French** (`lang="fr"`).
- Second language: **English** (`lang="en"`).
- Strategy: **single HTML file per page** with dynamic text replacement via `data-i18n` + JS, rather than separate HTML files per language. This simplifies maintenance but requires JS for full effect; include a `<noscript>` fallback that defaults to French content.
- All strings are stored in `/src/i18n/fr.json` and `/src/i18n/en.json`.
- Date, number, and currency formatting must use the `Intl` API (e.g., `new Intl.DateTimeFormat(locale).format(date)`).
- RTL support is not needed (FR and EN are both LTR), but structure CSS to not break if `dir="rtl"` is ever added.
- `hreflang` tags: use `<link rel="alternate" hreflang="x-default" href="/">` in addition to FR and EN alternates.

---

## 14. Fonts

- Self-host all fonts as `woff2` files under `/public/fonts/`.
- Load fonts with `<link rel="preload" as="font" type="font/woff2" crossorigin>` for the primary font.
- Use `font-display: swap` in `@font-face` declarations.
- Subset fonts to only the glyphs used (Latin + Latin Extended for FR/EN) using a tool like `pyftsubset` or `glyphhanger`.
- Provide a system font stack as a fallback that closely matches the chosen font's metrics to minimise CLS.

---

## 15. Images and Media

- Use `<picture>` element with `<source type="image/avif">`, `<source type="image/webp">`, and `<img>` fallback.
- Always specify `width` and `height` attributes matching the intrinsic size.
- Always provide meaningful `alt` text; for decorative images `alt=""`.
- Lazy-load all images below the fold with `loading="lazy"` and `decoding="async"`.
- The hero/LCP image must NOT be lazy-loaded; use `fetchpriority="high"` on it.
- Provide a `favicon.ico` (32×32) and `apple-touch-icon.png` (180×180) and a `site.webmanifest`.
- SVG icons: use an inline `<svg>` sprite (`<symbol>` elements in a hidden `<svg>` at the top of `<body>`) referenced with `<use href="#icon-name">`.

---

## 16. Commit and Code Style

- **Language**: All code comments and commit messages in English; all user-facing content has both FR and EN versions.
- **Commits**: Conventional Commits format — `feat:`, `fix:`, `style:`, `docs:`, `refactor:`, `perf:`, `a11y:`, `i18n:`, `security:`.
- **HTML**: 2-space indentation; double quotes for attributes.
- **CSS**: 2-space indentation; one property per line; properties sorted alphabetically within a rule; group related rules with blank-line separators.
- **JS**: 2-space indentation; single quotes for strings; `const` by default, `let` only when rebinding; no `var`; semicolons always; arrow functions for callbacks.
- **File names**: kebab-case for all files and directories.
- **No commented-out code** in committed files.

---

## 17. Copilot Behaviour in Agent Mode

When working on this project in Copilot Agent mode, you MUST:

1. **Always verify semantic correctness** of any HTML you generate — use the right element, not a `<div>` when a semantic element exists.
2. **Never add a framework or library** without explicit user approval; suggest vanilla alternatives first.
3. **Generate ARIA attributes proactively** — do not wait to be asked; every interactive component must be accessible from the start.
4. **Check colour contrast** when suggesting colour combinations; call out any combination that fails WCAG AA.
5. **Write mobile-first CSS** — start with the smallest viewport and add `min-width` media queries, never `max-width` overrides.
6. **Include bilingual content stubs** (`data-i18n` attributes + matching keys in both JSON files) whenever you create UI text.
7. **Suggest performance-conscious patterns**: `loading="lazy"`, `fetchpriority`, `preload`, `will-change` sparingly.
8. **Flag security concerns** immediately: any form, input, or fetch should be scrutinised.
9. **Propose the minimal JS solution** — if CSS can do it, use CSS.
10. **Do not generate placeholder/lorem ipsum** content; ask the user for the real text if unknown.
11. **Always include a `<noscript>` fallback** for any JS-dependent feature.
12. **Keep files small and focused** — split components early rather than creating monolithic files.

---

## 18. Testing and Quality Checks

Run the following checks before considering any feature complete:

| Tool | Command | Threshold |
|---|---|---|
| Lighthouse CI | `lhci autorun` | All scores ≥ 95 |
| axe-core (a11y) | `npx axe http://localhost:5173` | 0 violations |
| HTML validator | `npx html-validate "pages/**/*.html"` | 0 errors |
| CSS linter | `npx stylelint "src/css/**/*.css"` | 0 errors |
| JS linter | `npx eslint "src/js/**/*.js"` | 0 errors |
| Link checker | `npx broken-link-checker http://localhost:5173` | 0 broken links |

> These tools are dev dependencies and must be installed; they do not ship to production.

---

## 19. Environment Variables

Never hard-code secrets. The following environment variables are required by the contact form backend:

```
EMAIL_FROM=           # Sender address (e.g. noreply@yellowcraft.fr)
EMAIL_TO=             # Destination address
SMTP_API_KEY=         # Transactional email provider API key
CSRF_SECRET=          # Random 32-byte hex string for CSRF token signing
RATE_LIMIT_WINDOW=3600  # In seconds (default: 1 hour)
RATE_LIMIT_MAX=5        # Max submissions per window per IP
```

Store these in a `.env` file locally (add `.env` to `.gitignore`); on the server, inject them via the hosting platform's secrets manager.

---

## 20. Deployment

- The frontend is a collection of static HTML/CSS/JS files — deployable on any static host (Netlify, Vercel, Cloudflare Pages, or a simple VPS with nginx).
- The contact form backend is a single serverless function or a tiny HTTP server — deploy alongside or as a separate service.
- Configure the following HTTP response headers at the server/CDN level (not in HTML meta tags):
  - CSP (see §11)
  - HSTS
  - `X-Content-Type-Options: nosniff`
  - `X-Frame-Options: DENY`
  - `Referrer-Policy: strict-origin-when-cross-origin`
  - `Permissions-Policy`
- Enable Brotli compression.
- Configure `Cache-Control` per asset type (see §10).
- Redirect all `http://` to `https://` at the server level.
- Add a `404.html` and `500.html` custom error page.
