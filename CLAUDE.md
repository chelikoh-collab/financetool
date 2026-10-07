# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A one-page marketing site for "Horizon Wealth Planning" (a fictional financial planning firm). Everything lives in a single file, `index.html`, with CSS in one `<style>` tag and vanilla JS in one `<script>` tag. There is no framework, no build step, no package manager and no tests. Keep it that way: don't split the file or add dependencies unless asked.

## Running

Open the file directly in a browser:

```bash
open index.html
```

External resources load at runtime and need an internet connection: Google Fonts (Playfair Display for headings, Inter for body text), the Unsplash image behind the hero, and the `i.pravatar.cc` avatars.

## Structure of index.html

The file is organised into commented, numbered sections, and the numbering is parallel across all three languages. When adding a feature, add matching blocks in each place:

- **CSS:** design tokens → base → buttons → fade-in → nav → hero → carousel → contact → footer → responsive breakpoints → reduced motion.
- **HTML:** header/nav → hero (`#home`) → testimonials (`#testimonials`) → enquiry form (`#contact`) → footer → back-to-top button.
- **JS:** one `'use strict'` IIFE with sections for the year → nav → fade-in → count-up → carousel → enquiry form → newsletter → scroll handling.

### Conventions

- **Theming:** colours, spacing, radius, shadows and `--nav-h` are all CSS custom properties on `:root`. Use the tokens rather than hard-coded values. The brand colours are navy `#0B2545`, gold `#C9A227` and background `#F7F9FC`.
- **Mobile-first CSS:** base styles are for mobile, with `min-width` overrides at **768px** and **1024px** only.
- **Fade-in animation:** add the class `.fade-in` (optionally with `.delay-1`, `.delay-2` or `.delay-3`) to any element. An IntersectionObserver adds `.is-visible` once and then stops watching it.
- **Count-up stats:** these are driven by data attributes on `.stat-num`: `data-target`, `data-prefix` and `data-suffix`. A visually hidden sibling holds the final value for screen readers.
- **Carousel cards per view:** `perView()` in the JS uses `matchMedia` to return 1, 2 or 3 cards. It must stay in sync with the CSS `.slide` `flex-basis` values (100%, 50%, 33.333%).
  - The dots are generated one per scroll position (`slides − perView + 1`) and rebuilt when the breakpoint changes.
  - Off-screen slides get `aria-hidden` and `inert`.
  - Autoplay runs every 5s and pauses on hover, on focus and during touch.
- **Enquiry form validation:** the `validators` map is keyed by the field's `name`, and each validator returns an error string, or `''` when valid. Every field needs:
  - a matching `<span class="error-msg" id="{name}-error">`
  - an entry in `styleTarget()` if the red border belongs on a wrapper rather than the input (the radio group and the consent checkbox already have one).

  Fields validate on blur/change, and on input once touched. A valid submit shows the spinner for 1.5s, logs the data as JSON to the console, and swaps `#form-card` for a success panel. Any user input inserted into the page must go through `textContent`, never `innerHTML`.
- **Accessibility:** the page relies on semantic landmarks, `aria-expanded`/`aria-controls` on the hamburger, carousel roles (`aria-roledescription`, `tablist`), `aria-invalid`/`aria-describedby` on form fields, and a `prefers-reduced-motion` override. Preserve all of these when editing.

## Placeholder content

The address, phone number, email (`hello@horizonwealth.example`), social links (`href="#"`) and testimonials are dummy content.
