# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Marketing/landing page for **Luni**, a privacy-first baby tracker iOS app built for Swiss families. The companion iOS app lives in a sibling directory at `/Users/schmerzli/Code/baby-tracker/` (BabyTracker + Luni Xcode projects). Keep copy, pricing, and feature claims on this site in sync with what the app actually ships.

Live at https://getluni.ch (via `CNAME`).

## Stack & Deployment

- **Zero build system.** Static HTML/CSS/JS — no package manager, no bundler, no dependencies.
- **GitHub Pages** auto-deploys on push to `main`. Jekyll is disabled (`_config.yml` has `theme: null`).
- To preview locally, open `index.html` directly in a browser or run any static server (e.g. `python3 -m http.server`).
- No tests, no lint — verification is visual in-browser.

## Architecture

Single-page marketing site. All structure is in 4 flat HTML files at the repo root:

- `index.html` — landing page; **contains all styles and scripts inline** (one `<style>` block in `<head>`, one `<script>` at the bottom). No external CSS/JS files.
- `privacy.html`, `terms.html`, `eula.html` — legal pages.
- `screenshots/` — PNG assets referenced from the page. Note paths are **case-sensitive** (`.PNG` uppercase) since GitHub Pages serves from a case-sensitive filesystem — mismatches break images in production but may work locally on macOS.

### i18n system (index.html)

The site supports **4 languages: German (default), French, Italian, English**. The pattern:

- A `translations` object in the inline `<script>` holds nested key → string maps per locale.
- DOM nodes opt in via `data-i18n="some.key"` (and `data-i18n-placeholder`, etc. for attributes).
- On load, browser language is detected and mapped to a supported locale, falling back to German.
- A language picker in the nav swaps the active locale and re-applies translations across the DOM.

When adding copy: add the string to **all four** language blocks in `translations`, then reference it with `data-i18n` on the element. Don't hardcode user-visible text.

### Responsive design

Mobile-first with a primary breakpoint at 768px. The iPhone layout was recently fixed (see commit `64739bd`) — test mobile widths when touching hero, nav, or grid sections.

## Conventions

- Keep everything inline in `index.html` — don't split CSS/JS into separate files unless there's a strong reason. The single-file structure is intentional for a static marketing page.
- Swiss-specific content (U-Untersuchungen, CHF pricing, 41+ Swiss health reminders) is a core differentiator — preserve it in edits.
- Privacy messaging ("no servers, no analytics, no accounts") must stay truthful to the app's actual behavior (CloudKit/iCloud sync only).
