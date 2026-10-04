# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing site for **Austcham Paddle Club Singapore**, a dragon boat / outrigger canoe / single-craft paddling club on Kallang Basin (affiliated with the Australian Chamber of Commerce, est. 1988), implemented entirely in one static file: [index.html](index.html). There is no build step, no package manager, no framework, and no test suite — plain HTML/CSS/JS only. It's deployed via GitHub Pages.

## Running / previewing

There is nothing to install or build. Open the file directly in a browser:

```bash
open index.html
```

There is no linter or test runner configured in this repo. Verify changes by opening the file and manually exercising the page (see "Manual verification" below).

## Architecture

Everything lives in `index.html`, organized into three layers within that one file:

- **`<head>`**: SEO/meta tags (title, description, canonical, Open Graph, Twitter card) and a `SportsActivityLocation` JSON-LD schema block for the club, ahead of the inline `<style>` block.
- **HTML** (`<body>`): semantic sections in document order — sticky `<header>`/nav, then `<main>` containing `#hero`, `#race-board` (crew showcase with animated stat counters), `#activities` (swipeable sports carousel), `#why-us`, `#races` (scroll-driven 3D fly-through of recent race photos), `#challenge` (10km Challenge banner), `#testimonials`, `#join` (membership pricing "lanes" + student pricing strip), `#get-started`, `#training` (weekly session schedule), `#enquiry` (the enquiry form), `#faq`, and `#sponsors`, then `<footer id="contact">`. A floating WhatsApp chat widget and a confetti `<canvas>` sit after the footer, outside `<main>`. Each major section is marked with an HTML comment (e.g. `<!-- Hero Section -->`) to make it easy to jump to.
- **CSS** (inline `<style>` in `<head>`): mobile-first, using CSS custom properties on `:root` for the green/gold color palette. Layout breakpoints are `@media (min-width: 640/768/900/1024/1100px)`. Scroll-triggered reveals use a `.fade-in` / `.fade-in.visible` class pair driven by JS, not pure CSS. A `prefers-reduced-motion` block disables/simplifies animations (waterline drift, card tilt, races fly-through, buoy bob, etc.).
- **JavaScript** (inline `<script>` at the end of `<body>`): several independent behaviors, gated by a shared `prefersReducedMotion` check where relevant:
  1. Mobile nav toggle (hamburger button ↔ `#main-nav.open`)
  2. Scroll fade-in via `IntersectionObserver` over all `.fade-in` elements (one-time reveal, unobserved after trigger)
  3. Race-board stat counters (`.count-up`) — animated count-up on scroll into view, skipped (instant final value) under reduced motion
  4. Pointer-follow spotlight glow (`initSpotlight`) on the crew showcase and activities sections, and card tilt-on-hover (`.tilt`), both skipped without a fine pointer
  5. `#races` scroll-driven 3D fly-through — falls back to a static grid (`.races-static`) under reduced motion
  6. Crew showcase parallax/fade tied to scroll position
  7. Sports carousel (`#sport-deck`) — stacked-card deck navigable via prev/next buttons, dot indicators, or pointer drag/swipe
  8. Footer copyright year (`#year`, set from `new Date()`)
  9. Enquiry form validation/submission (`#enquiry-form` submit handler) — validates name/email/phone client-side, writes per-field errors into `<span class="error-msg" id="{field}-error">`. On success it POSTs to FormSubmit (`https://formsubmit.co/ajax/AustchamPaddleClubSG@gmail.com`, cc'ing `mybluecoffeecup@gmail.com` via the `_cc` field), logs the submitted `data` object to the console, shows a status message in the `aria-live` region, resets the form, and fires a confetti burst + spoken thank-you (`speechSynthesis`) — **this is a live integration that emails the club inbox, not a stub.**
  10. WhatsApp chat widget (`.whatsapp-widget`) — toggleable panel with canned quick-reply buttons that deep-link into `wa.me` with a prefilled message

Keep new code within this same three-layer structure (HTML in `<body>`, styles in the single `<style>` block, behavior in the single `<script>` block) rather than splitting into separate files, unless asked to change the project's build approach.

## Manual verification

When changing this file, check in a browser:
- Nav links smooth-scroll to their section; sticky header behaves correctly on scroll; hamburger menu opens/closes on narrow viewports.
- Layout at mobile (~375px) and desktop widths.
- Testimonial cards (and other `.fade-in` elements) fade in on scroll.
- Sports carousel: prev/next buttons, dot indicators, and drag/swipe all advance the deck and keep the dots in sync.
- `#races` fly-through scrolls smoothly and falls back to a static grid with reduced motion enabled.
- Enquiry form: submitting empty shows inline field errors (no `alert()`); submitting valid data shows the success message, resets the form, logs the submitted data object to the devtools console, and (careful — this hits a live endpoint) emails the club inbox via FormSubmit.
- WhatsApp widget opens/closes via its toggle, Escape, and outside click; quick-reply buttons open `wa.me` with the expected prefilled text.
- Footer year is correct.
- Full keyboard-only tab pass through nav, buttons, carousel controls, FAQ `<details>`, and form fields shows visible focus states.
