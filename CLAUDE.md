# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file static HTML portfolio/landing page for Oleksii Alekhneiko, a freelance chef (Mietkoch) available for hire in Germany. The entire site lives in `index.html.html` — no build system, no framework, no dependencies beyond CDN-loaded libraries.

## Development

There is no build step. Open `index.html.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080/index.html.html
```

No linter, test suite, or package manager is configured.

## Architecture

The file is structured in three self-contained blocks inside one HTML file:

1. **`<style>`** — All CSS using custom properties defined in `:root`. The color palette (gold/dark theme) is controlled entirely through these variables. Layout uses CSS Grid throughout.

2. **HTML body** — Six sections in order: `#hero`, `#profil`, `#skills`, `#erfahrung`, `#buchen`, `footer`. Navigation is fixed and uses anchor links to these IDs.

3. **`<script>`** — Vanilla JS at the bottom handles:
   - Nav scroll state (`.scrolled` class toggle)
   - Scroll-reveal animations via `IntersectionObserver` on `.reveal` elements
   - Date field minimum enforcement
   - Form validation and `emailjs.send()` submission with `mailto:` fallback on failure
   - Toast notifications via `showToast()`

## EmailJS integration

The contact form sends email via [EmailJS](https://www.emailjs.com). Three placeholder constants at the top of the script block must be replaced with real values before the form works:

```js
const EMAILJS_PUBLIC_KEY  = 'DEIN_PUBLIC_KEY';   // Account → API Keys
const EMAILJS_SERVICE_ID  = 'DEIN_SERVICE_ID';   // Email Services
const EMAILJS_TEMPLATE_ID = 'DEIN_TEMPLATE_ID';  // Email Templates
```

The target address is hardcoded as `aleks.alekh.25@gmail.com` in `templateParams.to_email` and the `mailto:` fallback.

## Key conventions

- **CSS custom properties** — never use raw color/spacing values; always reference a `var(--…)` token from `:root`.
- **Responsive breakpoints** — `@media (max-width: 900px)` for tablet, `@media (max-width: 600px)` for mobile. Grid layouts collapse to single-column at 900px.
- **Inline styles** — used sparingly for one-off overrides (e.g. `grid-column: span 2` on a single language card). Prefer class-based CSS for anything reused.
- **`.reveal` pattern** — add class `reveal` to any element that should fade-in on scroll; the `IntersectionObserver` in the script adds `visible` when the element enters the viewport. Stagger is automatic based on element index mod 4.
- **Language** — all user-visible text is in German (`lang="de"`). Keep new content in German.
