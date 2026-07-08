# Emerald Hawks FC

A single-page marketing site for a Singapore-based football club, built with plain HTML, CSS, and JavaScript — no frameworks, no build step, no dependencies.

**Live site:** https://mybluecoffeecup-ops.github.io/test2/

![Emerald Hawks FC homepage screenshot](assets/screenshot.png)

## Features

- Sticky, responsive navbar with a mobile hamburger menu
- Hero section with a scoreboard-style stats strip and a staggered entrance animation (respects `prefers-reduced-motion`)
- Pitch-line "kickoff mark" dividers between sections and a club crest badge, tying the visual identity to the sport
- Programs section listing all five squads (Mini Hawks through Adult League)
- Why Us trust section and a testimonials grid with scroll-triggered fade-in animations
- Trial enquiry form with client-side validation (name, email, phone), accessible inline error messages, and live submission via [FormSubmit](https://formsubmit.co/)
- Floating WhatsApp chat widget (bottom-right) with a pulsing call-to-action, suggested quick-reply questions, and deep links to `wa.me`
- SEO metadata: descriptive title/description, Open Graph & Twitter cards, canonical URL, and JSON-LD structured data
- Footer with contact details, quick links, and an auto-updating copyright year

## Getting started

There's nothing to install or build. Clone the repo and open the file directly in a browser:

```bash
git clone https://github.com/mybluecoffeecup-ops/test2.git
cd test2
open index.html
```

## Project structure

```
index.html   # entire site: HTML structure, inline <style>, inline <script>
```

Everything lives in one file, organized into three layers: HTML markup, a single `<style>` block, and a single `<script>` block at the end of `<body>`. See [CLAUDE.md](CLAUDE.md) for a full architecture breakdown.

## Deployment

Pushes to `main` automatically deploy to GitHub Pages via the workflow at [.github/workflows/deploy-pages.yml](.github/workflows/deploy-pages.yml).
