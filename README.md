# Yellow Craft — Company Showcase Website

> Official showcase website for **Yellow Craft**, a French software-development micro-business.

## What this project is

A lightning-fast, fully accessible, bilingual (French 🇫🇷 / English 🇬🇧) company showcase website built with:

- **Semantic HTML5** — no framework
- **Vanilla CSS3** — custom properties, flexbox/grid, `@layer`, mobile-first
- **Minimal vanilla JS** — only where CSS cannot do the job
- **A small backend** — contact-form handler only (serverless function or Node/Python/PHP)

## Goals

| Criterion | Target |
|---|---|
| Lighthouse Performance | ≥ 98 |
| Lighthouse Accessibility | ≥ 98 |
| Lighthouse Best Practices | ≥ 98 |
| Lighthouse SEO | ≥ 98 |
| WCAG | 2.1 Level AA (RGAA 4.1) |
| EAA | Compliant |

## Pages

| URL | Description |
|---|---|
| `/` | Home — hero, services, about, CTA |
| `/contact` | Contact form (secure, CSRF-protected, rate-limited) |
| `/mentions-legales` | Legal Notice (required by French LCEN) |
| `/politique-de-confidentialite` | Privacy Policy (GDPR/RGPD) |
| `/politique-de-cookies` | Cookie Policy (ePrivacy Directive) |
| `/accessibilite` | Accessibility Statement (RGAA) |
| `/plan-du-site` | Sitemap |

## Copilot instructions

See [`.github/copilot-instructions.md`](.github/copilot-instructions.md) for the full set of coding guidelines, architecture decisions, and AI-assistant rules used when developing this project in GitHub Copilot Agent mode.

## Development

```bash
# Install dev dependencies (linters, validators, Lighthouse CI)
npm install

# Start local dev server
npm run dev

# Production build
npm run build

# Run quality checks
npm run lint
npm run validate
npm run lighthouse
```

## Environment variables

Copy `.env.example` to `.env` and fill in your values before running the backend locally.
See section 19 of the Copilot instructions for the full list of required variables.

## Licence

All rights reserved — Yellow Craft © 2024