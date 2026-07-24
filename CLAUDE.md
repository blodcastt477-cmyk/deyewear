# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-page static website for D'EYEWEAR, an independent optician in SS2,
Petaling Jaya (real business: No 40, Mezzanine Floor, Jalan SS2/67, SS 2,
47300 Petaling Jaya; phone 03-7877 9576; WhatsApp +60 17-331 9576;
info@deyewear.com; open daily 12–7 pm). Frame prices on the page are
placeholders, not confirmed by the owner.

The site began as `optical.html` in the `blodcastt477-cmyk/claude-code-first-pr`
repo and was split out here to get its own domain.

## Deployment

GitHub Pages serves the `main` branch root at **https://deyewear.store**
(the committed `CNAME` file; HTTPS enforced). Pushing to `main` redeploys in
a minute or two. Keep business details in the page consistent with the
JSON-LD block in `<head>` and with the Google Business Profile.

## Running Locally

No build, lint, or test tooling — plain HTML/CSS/JS with no dependencies.
Binary assets are the four frame photos in `images/` (compressed JPEGs);
everything else is inline SVG. Serve over HTTP (e.g.
`python -m http.server 5500`) rather than opening the file directly, since it
loads Google Fonts and the photos by relative path.

## Architecture

Everything is in `index.html`: CSS in a single `<style>` block in `<head>`
(SEO meta + JSON-LD are also in `<head>`), markup, then all JS in one
`<script>` block at the end of `<body>`. Keep this single-file structure —
don't split out CSS/JS files.

- Colors in `:root`: ink on cool optical-white, with a green→violet
  "anti-reflective coating flare" gradient (`--flare`) as the only accent.
  Fonts: Bricolage Grotesque (display), Instrument Sans (body), Fragment Mono
  (prescription-style data).
- Signature interaction: the hero headline is blurred with chromatic fringing;
  a cursor-following lens (`.stage-sharp` clipped to a circle + `.lens-ring`)
  reveals sharp text. Blur and sharp layers duplicate the same copy — edit both.
  Touch devices get an auto-drifting lens; reduced motion disables the effect
  via the `.lens-off` class.
- Other graphic set pieces: prescription ticker, frame photo cards
  (`.frame-photo`, ken-burns `frameDrift` animation with staggered delays)
  listing real temple measurements (e.g. `47□22–145`), lens cross-section
  diagram, Snellen eye chart where copy shrinks by acuity row, red/green
  duochrome strip.
- WhatsApp CTAs (hero and Visit section) link to `wa.me/60173319576` with a
  pre-filled message.

The page supports `prefers-reduced-motion` in CSS and JS, uses
IntersectionObserver-driven `.reveal` → `.in` scroll fades, and keeps
`:focus-visible` outlines. Preserve these when adding animated or interactive
features.
