# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Single-page, Spanish-language baby shower invitation for "Daziel", styled as an 8-bit / Sakura-themed game title screen. No build step, no package manager, no tests, no linter. Static site: open `index.html` directly or serve the folder (e.g. `python3 -m http.server`).

## Layout

- `index.html` — the entire app: inline `<style>`, markup, and one inline IIFE `<script>` (vanilla JS, no dependencies). All edits go here.
- `daziel.mp4` — video loaded by the "¿Por qué Daziel?" panel (`src="daziel.mp4"`, `preload="metadata"`).
- `sakura-logo.svg` — card back face image.
- `sakura-baby-shower-invite.zip` — bundled copy of assets; not referenced by the page. Does not auto-update when `index.html` changes.
- Only external dependency: Google Fonts (`Press Start 2P`, `DotGothic16`) via `<link>`.

## Architecture notes

- Palette and fonts are CSS custom properties in `:root`; use them rather than hardcoding colors.
- Page is one centered column (`.screen`, max 460px, mobile-first): HUD bar → flip card → dialog box → logo/title → accordion `.menu` → footer. Falling-petal `<canvas id="petals">` sits fixed behind it.
- Accordion: each `.item > button` toggles its panel via `aria-expanded` + the `hidden` attribute (panel id from `aria-controls`); arrow keys move focus between buttons. A new menu entry needs a matching `button`/`.panel` pair with consistent `id`/`aria-controls`/`aria-labelledby`. CSS has `[hidden]{display:none !important}`.
- Card flip is driven by `aria-pressed` on `#card` (CSS selector `.card[aria-pressed="true"] .flip`).
- Petal canvas animation is skipped entirely when `prefers-reduced-motion: reduce`; CSS animations are also disabled in that media query. Keep this when adding animation.
- The `return` on the reduced-motion/no-canvas check is inside the IIFE, so any new JS must go *above* the petal code to run in that case.

## Content that must be kept in sync

Event details appear in several places in `index.html`: date/time in `<meta name="description">`, `og:description`, and the "Iniciar misión" panel; the WhatsApp number `59179356587` appears in the RSVP panel text (`+591 79356587`) and in two `wa.me` links (RSVP and "Misión secundaria", with URL-encoded prefilled messages). The map link is `#addr` and the "Copiar enlace del mapa" button copies that element's `href`.
