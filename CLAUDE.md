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

## Case Study Page Pattern

Each case study (`zuora.html`, `oracle.html`, `workspan.html`) follows an identical section sequence: hero → situation → role → decisions → outcome → case-nav footer.

**Decision item thumbnails** use a `.has-thumb` modifier class on `.decision-item` that shifts the CSS grid from `80px 1fr` to `80px 350px 1fr`. The thumbnail is a `<button class="decision-thumb" data-full="…">` that opens a fullscreen image modal on click. Mobile (≤768px) collapses to single-column.

**Image modal** pattern: a fixed `<div class="image-modal" id="image-modal">` with open/close driven by `.open` class + `body.modal-open`. Close via × button, backdrop click, or Escape key. Modal HTML and JS are duplicated per page (no shared JS file).

**Scroll reveal:** `IntersectionObserver` adds `.visible` to `.reveal` elements as they enter the viewport (staggered 60ms delay). Defined inline in each page's `<script>` block.

**Role section image:** `.role-image-wrap` with a `::after` pseudo-element creates the offset border effect (10px offset, `var(--rule)` border).

## Image Assets

- `images/Zuora_Screens/` — Zuora product screenshots + Occam design system
- `images/Responsys_oracle/` — Oracle/Responsys screenshots (mixed formats: png, webp, jpeg)
- `images/Workspan/` — WorkSpan screenshots (all png, PascalCase filenames)
- `images/zuora-logo.svg` — only logo file; Oracle and WorkSpan have no logos

## Case-Nav Chain

Homepage order matches the case-nav prev/next chain: Zuora → Oracle → WorkSpan. WorkSpan has no "next" link (right side hidden with `visibility:hidden`).

## Font Loading

Google Fonts loaded per-page (not in `styles.css`): `Cormorant+Garamond:ital,wght@0,500;0,600;0,700;1,500` and `DM+Sans:wght@500;700`. All `font-weight` values in the codebase are ≥ 500.
