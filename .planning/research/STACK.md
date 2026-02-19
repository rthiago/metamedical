# Technology Stack

**Project:** MetaMedical Visual Polish
**Researched:** 2026-02-18

## Recommended Stack

### Scroll Reveal Animations

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Vanilla JS + IntersectionObserver | Native API | Detect elements entering viewport | Zero dependencies, 97%+ browser support (since March 2019), runs asynchronously off main thread. For a 5-page static site, a library is overkill. 30 lines of custom JS replaces an entire library. |
| CSS transitions/transforms | Native | Animate the actual reveal effects | GPU-accelerated, declarative, respects `prefers-reduced-motion` natively via media query. Separation of concerns: JS detects, CSS animates. |

**Decision: Custom vanilla JS over AOS or ScrollReveal.**

Rationale:
- **AOS (v2.3.1, MIT, ~14KB)**: Stable but unmaintained since 2019. The `next` branch never shipped a v3 release. Adds CSS + JS dependency for what amounts to toggling a class. For 5 pages with ~15-20 animated elements, this is dependency overhead for no gain.
- **ScrollReveal (v4.0.9, GPL/Commercial, 5.7KB)**: GPL license requires your site source to be open, or you must buy a commercial license. Not worth it for class-toggling.
- **CSS scroll-driven animations (`animation-timeline: scroll()`)**: 78% browser support -- Firefox still behind a flag. NOT ready for production without polyfill. Revisit in 2027.
- **GSAP ScrollTrigger (48KB)**: Massive overkill for simple fade/slide reveals on a corporate site.

The vanilla approach: IntersectionObserver watches elements with a `data-reveal` attribute. When intersecting, add a `.revealed` class. CSS handles the transition. Total JS: ~30 lines. Total CSS: ~40 lines.

### Image Overlay Sections

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| CSS `::before` / `::after` pseudo-elements | Native | Dark/gradient overlays on images | Already used in `.hero::before`. Extend the pattern. Zero JS needed. |
| CSS `linear-gradient()` + `background-image` | Native | Gradient overlays combined with background images | Single CSS rule: `background: linear-gradient(...), url(...)`. No extra DOM elements. |
| CSS `object-fit` + `position: relative/absolute` | Native | Overlay text on `<img>` elements | For product cards where `<img>` is in the DOM (not a background). Wrap in positioned container, overlay with pseudo-element or positioned child. |

**Decision: Pure CSS using pseudo-elements and gradient backgrounds.**

The site already uses this pattern in `.hero::before`. Extend it to new sections (product detail pages, about section). No new technology needed.

### Enhanced Typography

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| CSS `clamp()` | Native | Fluid responsive font sizing | 96%+ browser support. Replaces media query breakpoints for font sizes. One line instead of three `@media` rules. |
| Google Fonts (existing) | CDN | Additional font weights | Already loading Inter (400,500,600) and Montserrat (600,700). Add Montserrat 800/900 for bold hero typography. No new dependency -- just expand the existing `?family=` parameter. |
| CSS `letter-spacing` + `text-transform` | Native | Typographic refinement | Subtitle treatments, section labels. Already partially used in `.product-subtitle`. |

**Decision: CSS `clamp()` for fluid sizing, expand existing Google Fonts weights.**

Implementation: Replace fixed `font-size` values in `:root` with `clamp()` equivalents. Example: `h1 { font-size: clamp(2rem, 1.5rem + 2.5vw, 3.5rem); }`.

### Product Grid Enhancements

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| CSS Grid (`auto-fit` / `minmax()`) | Native | Responsive product grid | Already in use (`.products-grid`). Enhance with better `minmax()` values and gap refinements. |
| CSS `aspect-ratio` | Native | Consistent image dimensions | 95%+ browser support. Already used on `.product-card img`. Standardize across all product images. |
| CSS `:hover` transforms | Native | Interactive card effects | Already partially implemented. Enhance with overlay effects on hover. |

**Decision: Refine existing CSS Grid. No new technology.**

The existing grid structure is sound. Improvements are CSS refinements: better `minmax()` breakpoints, hover overlay effects on product images, consistent `aspect-ratio` usage.

## Complete Stack Summary

| Layer | Technology | Status |
|-------|-----------|--------|
| Structure | HTML5 | Existing -- no changes |
| Styling | CSS3 (custom properties, grid, clamp, transitions) | Existing -- extend |
| Fonts | Google Fonts CDN (Inter + Montserrat) | Existing -- add weight 800 |
| Scroll Detection | Vanilla JS IntersectionObserver | **NEW** -- ~30 lines |
| Animations | CSS transitions + transforms | Existing -- extend |
| Image Overlays | CSS pseudo-elements + gradients | Existing pattern -- extend |
| Build Tools | None | Unchanged |

## What NOT to Add

| Technology | Why Not |
|------------|---------|
| AOS library | Unmaintained since 2019. Adds 14KB for class-toggling a 5-page site can do in 30 lines. |
| ScrollReveal | GPL license problematic for commercial site. Paid alternative exists but unnecessary. |
| GSAP / ScrollTrigger | 48KB+ for simple fade/slide effects. Designed for complex timeline animations this site does not need. |
| CSS `animation-timeline: scroll()` | Firefox behind flag. 78% support. Not production-ready without polyfill. |
| Tailwind / any CSS framework | Existing custom CSS is clean and well-structured. Adding a framework would be a rewrite, not an enhancement. |
| Any build tool (Vite, Webpack) | Site is 5 static HTML files on shared hosting. Build tools add complexity with zero benefit here. |
| jQuery | Dead weight. IntersectionObserver is native and more performant. |

## New File Structure

```
metamedical/
  css/style.css          -- Extend with reveal animations, overlays, clamp() typography
  js/reveal.js           -- NEW: ~30 lines, IntersectionObserver scroll reveal
  index.html             -- Add <script src="js/reveal.js"> + data-reveal attributes
  empresa.html           -- Same
  produtos.html          -- Same
  localizacao.html       -- Same
  contato.html           -- Same
```

## Implementation Reference

### Scroll Reveal JS (~30 lines)

```javascript
// js/reveal.js — Scroll reveal using IntersectionObserver
(function () {
  // Respect user motion preferences
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    // Make all elements visible immediately, skip animations
    document.querySelectorAll('[data-reveal]').forEach(function (el) {
      el.classList.add('revealed');
    });
    return;
  }

  var observer = new IntersectionObserver(function (entries) {
    entries.forEach(function (entry) {
      if (entry.isIntersecting) {
        entry.target.classList.add('revealed');
        observer.unobserve(entry.target); // Animate once only
      }
    });
  }, {
    threshold: 0.15,
    rootMargin: '0px 0px -50px 0px'
  });

  document.querySelectorAll('[data-reveal]').forEach(function (el) {
    observer.observe(el);
  });
})();
```

### Scroll Reveal CSS

```css
/* Scroll reveal base state */
[data-reveal] {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

[data-reveal="left"] {
  transform: translateX(-30px);
}

[data-reveal="right"] {
  transform: translateX(30px);
}

[data-reveal="scale"] {
  transform: scale(0.95);
}

/* Revealed state */
[data-reveal].revealed {
  opacity: 1;
  transform: translate(0) scale(1);
}

/* Staggered delays for grid children */
[data-reveal-delay="1"] { transition-delay: 0.1s; }
[data-reveal-delay="2"] { transition-delay: 0.2s; }
[data-reveal-delay="3"] { transition-delay: 0.3s; }
[data-reveal-delay="4"] { transition-delay: 0.4s; }

/* Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  [data-reveal] {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

### Fluid Typography with clamp()

```css
/* Replace fixed sizes in :root or element rules */
h1 { font-size: clamp(2rem, 1.5rem + 2.5vw, 3.5rem); }
h2 { font-size: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem); }
h3 { font-size: clamp(1.25rem, 1rem + 1vw, 1.75rem); }
.hero h1 { font-size: clamp(2.25rem, 1.5rem + 3.5vw, 4rem); }
.tagline { font-size: clamp(1rem, 0.85rem + 0.75vw, 1.5rem); }
```

### Image Overlay Pattern

```css
/* Reusable overlay on any section with background image */
.overlay-section {
  position: relative;
  background: center/cover no-repeat;
  color: var(--color-white);
}

.overlay-section::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(0, 56, 179, 0.7),
    rgba(0, 30, 80, 0.85)
  );
  z-index: 1;
}

.overlay-section > * {
  position: relative;
  z-index: 2;
}

/* Product card image hover overlay */
.product-card {
  position: relative;
  overflow: hidden;
}

.product-card img {
  transition: transform 0.4s ease;
}

.product-card:hover img {
  transform: scale(1.05);
}
```

### Google Fonts Weight Addition

```html
<!-- Current -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Montserrat:wght@600;700&display=swap" rel="stylesheet">

<!-- Updated: add Montserrat 800 for bold hero text -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Montserrat:wght@600;700;800&display=swap" rel="stylesheet">
```

## Accessibility Requirements

| Requirement | Implementation |
|-------------|---------------|
| `prefers-reduced-motion: reduce` | CSS media query disables all transitions. JS checks `matchMedia` and skips observer setup. |
| No content hidden by default | `[data-reveal]` elements set to `opacity: 0` but become visible on intersection. For no-JS fallback, add `<noscript>` style making all revealed. |
| No-JS fallback | Add `<noscript><style>[data-reveal]{opacity:1;transform:none}</style></noscript>` to `<head>`. |
| WCAG 2.1 SC 2.3.3 | Animations triggered by scrolling (not auto-playing). User controls scroll speed. Reduced motion respected. |

## Sources

- [IntersectionObserver MDN](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver) — HIGH confidence
- [IntersectionObserver Can I Use — 97%+ support](https://caniuse.com/intersectionobserver) — HIGH confidence
- [CSS clamp() MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/clamp) — HIGH confidence
- [CSS scroll-driven animations MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations) — HIGH confidence (for the "not ready" assessment)
- [animation-timeline: scroll() Can I Use — 78%](https://caniuse.com/mdn-css_properties_animation-timeline_scroll) — HIGH confidence
- [AOS GitHub — MIT, last stable v2.3.1](https://github.com/michalsnik/aos) — HIGH confidence
- [ScrollReveal — GPL/Commercial](https://scrollrevealjs.org/guide/installation.html) — HIGH confidence
- [prefers-reduced-motion MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion) — HIGH confidence
- [Smashing Magazine — Modern Fluid Typography with clamp()](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/) — MEDIUM confidence
- [CSS image overlay techniques](https://ishadeed.com/article/handling-text-over-image-css/) — MEDIUM confidence
- [W3C WCAG 2.1 SC 2.3.3 Animation from Interactions](https://www.w3.org/WAI/WCAG21/Understanding/animation-from-interactions.html) — HIGH confidence
