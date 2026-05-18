# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Dev Server

```bash
python3 -m http.server 8765
```

Preview at `localhost:8765`. The `.claude/launch.json` configures this for the Claude Code preview tools (`serverId: "portfolio"`).

## Architecture

Vanilla HTML/CSS/JS — no build tools, no frameworks, no dependencies.

**Pages:** `index.html` (homepage), `zuora.html`, `oracle.html`, `workspan.html` (case studies)

**Shared styles:** `styles.css` — base reset, CSS custom properties, nav, buttons, tags, `.reveal` scroll animation, responsive breakpoints. All page-specific styles live in an inline `<style>` block within each HTML file.

**CSS custom properties** (defined in `styles.css :root`):
- Colors: `--ink`, `--ink-light`, `--ink-faint`, `--paper`, `--paper-warm`, `--accent`, `--accent-light`, `--rule`
- Fonts: `--serif` (Cormorant Garamond), `--sans` (DM Sans)
- Layout: `--max` (1100px), `--gutter`

## Typography Rules

- All `font-weight` values are ≥ 500 throughout the codebase.
- Google Fonts loaded per-page (not in `styles.css`): `Cormorant+Garamond:ital,wght@0,500;0,600;0,700;1,500` and `DM+Sans:wght@500;700`.
- `.section-label` (in `styles.css`): `font-size: 18px`, accent colour, uppercase — used as the standard eyebrow label across all sections and pages.
- Body copy uses DM Sans at 0.92rem / line-height 1.7 (`--ink-light`). The `.role-body p:first-child` override has been removed — all paragraphs in role and situation sections share the same style.
- Em dashes (`—`) are not used in copy. Replacements: commas for asides, semicolons for related independent clauses, colons before elaborations.

## Homepage (`index.html`) Structure

- **Hero**: animated fade-in, hamburger nav on mobile (≤768px).
- **Selected Work** (case study list): `.case-study` rows use `grid-template-columns: 80px 1fr auto`. Zuora, Oracle, and WorkSpan rows use the `.has-thumb` modifier (`grid-template-columns: 80px 220px 1fr auto`) to add a screenshot thumbnail.
- **About**, **Philosophy**, **Writing**, **Contact** sections follow below.
- Writing articles (`.art-title`) use the same font-size/line-height as `.case-title`; subtitles (`.art-sub`) match `.case-desc`.

## Hero Layout (`index.html`)

The hero uses a two-part structure inside `.hero-grid`:

1. **`.hero-name-row`** — `display: flex; justify-content: space-between; max-width: 680px`
   - `.hero-name` (h1): `text-align: right` — text is right-justified within its natural block width; the block itself is left-aligned. "Steven" and "Travis" are on separate lines via `<br>`.
   - `.hero-portrait-wrap`: `margin-right: 30px` nudges the portrait 30px left from the right edge of the 680px row.

2. **`.hero-below`** — tagline, sub-text, CTAs, stats; `width: 100%`
   - `.hero-tagline`: `max-width: 680px` — matches the name-row width so text wraps at the same right edge as the portrait.
   - `.hero-sub`: `max-width: 680px`

**Mobile (≤768px):** `.hero-name-row` stays `flex-direction: row; align-items: flex-start`. Portrait is `width: 112px` (sized to match the rendered height of the name block).

## Nav (`styles.css`)

`.nav-logo` uses `var(--sans)` (DM Sans) at `0.8rem`, `font-weight: 500`, `letter-spacing: 0.1em` — matching the nav link style but without `text-transform: uppercase` (mixed case preserved).

## Mobile Nav

Hamburger nav activates at ≤768px. The `<ul id="primary-nav">` slides down from `top: 60px`. The closed state uses `transform: translateY(calc(-100% - 60px))` to push the drawer fully off-screen (accounting for the 60px `top` offset so the bottom edge lands at y=0). The toggle button uses `aria-expanded` with a bars-to-X CSS animation. Clicking any nav link or outside the drawer closes it.

## Case Study Page Pattern

Each case study follows an identical section sequence: **hero → situation → role → decisions → outcome → case-nav footer**.

**Hero brand row**: each case study has a `.case-hero-brand` flex row (logo + `.case-hero-brand-name` text) between the back-link and the eyebrow. Logos: `images/zuora-logo.svg`, `images/Oracle.png`, `images/workspan.png`. All logos render at 36px height, 55% opacity, desaturated (`filter: saturate(0.4)`).

**Decision item thumbnails** use `.has-thumb` on `.decision-item`, shifting the grid from `80px 1fr` to `80px 350px 1fr`. The thumbnail is a `<button class="decision-thumb" data-full="…">` that opens a fullscreen modal. Mobile (≤768px) collapses to single-column.

**Decision title links**: `.decision-title a` uses `color: var(--accent)` with no underline (underline on hover). Used on the Zuora "Occam Design System" link to `occam.zuora.com`.

**Image modal**: fixed `<div class="image-modal" id="image-modal">`, toggled via `.open` class + `body.modal-open`. Closes via × button, backdrop click, or Escape. Modal HTML and JS are duplicated per page (no shared JS file).

**Role section image**: `.role-image-wrap` with a `::after` pseudo-element creates a 10px offset border effect.

**Scroll reveal**: `IntersectionObserver` adds `.visible` to `.reveal` elements (staggered 60ms). Defined inline in each page's `<script>` block.

## Image Assets

- `images/Zuora_Screens/` — Zuora screenshots + Occam design system (png)
- `images/Responsys_oracle/` — Oracle/Responsys screenshots (mixed: png, webp, jpeg; **no avif** despite directory listing — use `.png`)
- `images/Workspan/` — WorkSpan screenshots (all png, PascalCase filenames e.g. `Home.png`, `Table_View.png`)
- `images/zuora-logo.svg` — Zuora logo
- `images/Oracle.png` — Oracle logo
- `images/workspan.png` — WorkSpan logo

## Case-Nav Chain

Zuora → Oracle → WorkSpan. WorkSpan's right link is hidden with `visibility: hidden`. When adding a new case study, update both the new page's nav and its neighbour's nav.

## iOS / Mobile

This is a regular website (not a PWA), so the viewport meta tag uses the standard `width=device-width, initial-scale=1.0` and the nav does not compensate for the iOS notch / Dynamic Island — the browser's own chrome already reserves that space.
