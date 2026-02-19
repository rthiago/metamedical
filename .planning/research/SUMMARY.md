# Research Summary: MetaMedical Visual Polish

**Domain:** Visual polish enhancements for static medical device website
**Researched:** 2026-02-18
**Overall confidence:** HIGH

## Executive Summary

The MetaMedical site needs scroll reveal animations, image overlay sections, enhanced typography, and product grid refinements. The existing stack (plain HTML, CSS with custom properties, Google Fonts CDN) is well-structured and requires minimal additions. The only new technology is a single vanilla JavaScript file (~30 lines) using the IntersectionObserver API for scroll-triggered class toggling.

All visual effects -- fade/slide animations, image overlays, fluid typography, hover effects -- are achievable with pure CSS transitions and transforms. The existing codebase already demonstrates the key patterns (hero overlay with `::before`, card hover transforms, CSS Grid layouts). This milestone is about extending and refining those patterns, not introducing new paradigms.

Third-party animation libraries (AOS, ScrollReveal, GSAP) were evaluated and rejected. AOS is unmaintained since 2019. ScrollReveal has GPL licensing concerns for a commercial site. GSAP is oversized for simple reveals. The native IntersectionObserver API has 97%+ browser support and provides the same core capability (viewport detection) that these libraries wrap. CSS native scroll-driven animations (`animation-timeline: scroll()`) were also evaluated but rejected due to Firefox lacking support (behind a flag only, even through Firefox 150), making them unsuitable for production without a polyfill.

The biggest risk is accessibility: scroll animations must respect `prefers-reduced-motion`, and a no-JS fallback must prevent invisible content. Both are addressed with specific CSS media queries and `<noscript>` fallback styles.

## Key Findings

**Stack:** Zero new dependencies. One ~30-line vanilla JS file + CSS extensions to existing stylesheet. Add Montserrat 800 weight to Google Fonts link.
**Architecture:** HTML declares animations via `data-reveal` attributes. JS detects viewport entry. CSS handles all visual transitions. Clean separation of concerns.
**Critical pitfall:** Flash of invisible content (FOIC) if JS fails -- mitigated with `<noscript>` fallback and keeping above-fold content unanimated.

## Implications for Roadmap

Based on research, suggested phase structure:

1. **Foundation: Typography + Spacing** - Pure CSS changes, zero risk
   - Addresses: Fluid typography with `clamp()`, section spacing, card styling refinements, Google Fonts weight addition, hero gradient overlay upgrade
   - Avoids: No JS involved, cannot break anything. Ship immediately for visual lift.

2. **Scroll Reveal System** - Add JS file + CSS animation states + accessibility fallbacks
   - Addresses: IntersectionObserver setup, `[data-reveal]` CSS classes, `prefers-reduced-motion` handling, `<noscript>` fallback, hero entrance animation
   - Avoids: Build and test on one page before applying to all five

3. **Image Overlays + Card Polish** - CSS overlay patterns on sections and product cards
   - Addresses: Branded gradient overlays, product card hover effects (image zoom), section visual hierarchy
   - Avoids: Test contrast/readability with actual metamedical.com.br images before finalizing overlay opacity

4. **Integration + Refinement** - Apply across all pages, stagger timing, final polish
   - Addresses: Consistent `data-reveal` placement across all 5 pages, staggered grid animations, cross-page consistency audit
   - Avoids: Over-animating -- limit to 4-6 reveals per page

**Phase ordering rationale:**
- Typography and spacing are pure CSS, zero risk, and affect every page. Do them first so subsequent work builds on the improved visual baseline.
- Scroll reveal system is the single most impactful feature but involves the only new JS file. Isolate it in its own phase for clean testing, especially the accessibility fallbacks.
- Image overlays depend on spacing being correct (overlay sections need proper padding). Do after spacing is finalized.
- Integration last because it touches all 5 HTML files and benefits from having all CSS/JS finalized first.

**Research flags for phases:**
- Phase 2 (Scroll Reveal): Test no-JS fallback and `prefers-reduced-motion` behavior carefully. Standard pattern but testing is critical.
- Phase 3 (Image Overlays): Must test with actual product images from metamedical.com.br to verify contrast. Cannot fully validate with placeholders.
- Phases 1 and 4: Standard CSS patterns, unlikely to need additional research.

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | All technologies are native browser APIs (IntersectionObserver, CSS clamp, CSS transforms). Browser support verified via Can I Use. |
| Features | HIGH | Feature set is well-established web patterns. No novel techniques required. |
| Architecture | HIGH | Data-attribute-driven reveal pattern is widely documented and battle-tested across the industry. |
| Pitfalls | HIGH | Accessibility requirements verified against W3C WCAG 2.1. FOIC mitigation is standard practice. |

## Gaps to Address

- Actual product image brightness/contrast needs testing with real images to calibrate overlay opacity values
- Whether the shared hosting environment has Content-Security-Policy headers that might block the new JS file (unlikely but worth a quick check)
- The Montserrat 800 weight must be verified to render correctly at large display sizes -- check that it does not appear too thick on low-DPI screens
