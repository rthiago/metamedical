# Domain Pitfalls

**Domain:** Visual polish for static HTML/CSS medical device site
**Researched:** 2026-02-18

## Critical Pitfalls

### Pitfall 1: Flash of Invisible Content (FOIC)

**What goes wrong:** Elements with `data-reveal` start at `opacity: 0`. If JS fails to load, is blocked, or errors out, content stays invisible forever. Users see blank sections.

**Why it happens:** No fallback for JS failure. Aggressive initial hiding without safety net.

**Consequences:** Content completely invisible to users. SEO unaffected (crawlers read DOM), but real visitors see nothing below the hero.

**Prevention:**
1. Add `<noscript><style>[data-reveal]{opacity:1;transform:none}</style></noscript>` in `<head>` of every page.
2. Keep critical above-the-fold content (hero, nav, first section heading) OUT of `data-reveal`. Only animate below-fold content.
3. Consider a CSS-only timeout: a `@keyframes` that forces `opacity: 1` after 3 seconds, overridden by `.revealed` class when JS succeeds.

**Detection:** Disable JS in browser DevTools. If any content is invisible, the fallback is broken.

### Pitfall 2: Motion Sickness / Accessibility Violations

**What goes wrong:** Users with vestibular disorders experience nausea, dizziness, or disorientation from scroll-triggered animations.

**Why it happens:** Not respecting `prefers-reduced-motion` OS setting. Too many simultaneous animations. Excessive motion distance.

**Consequences:** Accessibility violation (WCAG 2.1 SC 2.3.3). Potential legal liability. Bad user experience for ~5% of population. Especially relevant for a medical site -- healthcare audience includes people with motion sensitivity.

**Prevention:**
1. CSS: `@media (prefers-reduced-motion: reduce) { [data-reveal] { opacity: 1; transform: none; transition: none; } }`
2. JS: Check `window.matchMedia('(prefers-reduced-motion: reduce)')` before setting up observer.
3. Keep animation distances small (30px max translateY, not 100px).
4. Keep animation durations short (0.4s-0.6s, not 1.5s).
5. Limit to ~4-6 animated elements per page, not every element.

**Detection:** Enable "Reduce motion" in OS settings. Verify site is fully usable with zero animations.

### Pitfall 3: Content Layout Shift (CLS) from Animations

**What goes wrong:** Animated elements cause cumulative layout shift that degrades Core Web Vitals scores.

**Why it happens:** Using CSS properties that affect layout (height, margin, padding) instead of compositor-only properties (opacity, transform).

**Consequences:** Poor CLS score in Google PageSpeed Insights. Potential ranking impact. Visual jumpiness for users.

**Prevention:**
- Use ONLY `opacity` and `transform` for animations. Never animate `height`, `margin`, or `padding`.
- `transform: translateY()` moves the element visually but does NOT affect document flow, so it does not cause CLS.
- Test with Chrome DevTools > Performance > Layout Shifts overlay after implementing.

**Detection:** Run Lighthouse audit. Check CLS score. Anything above 0.1 needs investigation.

## Moderate Pitfalls

### Pitfall 4: Over-Animating Everything

**What goes wrong:** Every paragraph, heading, and image animates on scroll. The site feels like a PowerPoint presentation circa 2005. Undermines the professional, trustworthy tone a medical device company needs.

**Prevention:**
- Animate section-level containers or card groups, not individual text blocks.
- Aim for 4-6 reveal animations per page, not 20.
- Hero section should NOT use scroll reveal (it is above fold -- show immediately). Use CSS `@keyframes` for hero entrance instead.
- Rule of thumb: animate containers, not their children individually.

### Pitfall 5: Inconsistent Animation Timing

**What goes wrong:** Different elements use different durations, easings, and delays randomly. Feels chaotic rather than designed.

**Prevention:**
- Define animation variables in `:root`: `--reveal-duration: 0.6s`, `--reveal-easing: ease`.
- Use ONE timing profile for all scroll reveals. Vary only with stagger delay, not duration or easing.
- Consistent easing: `ease` or `ease-out` for reveals. Never `linear` (feels mechanical) or `ease-in` (feels heavy).

### Pitfall 6: clamp() Font Sizes Too Aggressive

**What goes wrong:** Using large viewport units (e.g., `clamp(1rem, 5vw, 3rem)`) makes text resize noticeably while user drags browser window. Feels unstable. Can also fail WCAG SC 1.4.4 if max/min ratio exceeds 2.5x.

**Prevention:**
- Keep the viewport component small: `0.5vw` to `2.5vw` is the sweet spot.
- Keep min:max ratio under 2.5x (e.g., `clamp(1rem, ..., 2rem)` = 2x ratio, safe).
- Test by resizing browser from 320px to 1400px. Text should scale smoothly without feeling "alive."

### Pitfall 7: Image Overlay Contrast Insufficient

**What goes wrong:** White text over image overlay is unreadable because the overlay is too transparent or the source image has bright spots that bleed through.

**Prevention:**
- Minimum overlay opacity: `rgba(0,0,0,0.55)` for dark overlays (already used in hero).
- Test with actual images from metamedical.com.br, not placeholders. Some images have bright medical equipment that bleeds through thin overlays.
- Always verify with WCAG contrast checker. Need 4.5:1 minimum for normal text, 3:1 for large text.

## Minor Pitfalls

### Pitfall 8: Forgetting defer on Script Tag

**What goes wrong:** Script runs before DOM is ready. `querySelectorAll('[data-reveal]')` returns empty NodeList. No elements animate.

**Prevention:** Always use `<script src="js/reveal.js" defer></script>`. The `defer` attribute guarantees execution after HTML parsing.

### Pitfall 9: Google Fonts Weight Not Loading

**What goes wrong:** Adding Montserrat 800 weight in CSS but forgetting to update the Google Fonts `<link>` URL. Browser silently falls back to nearest available weight (700), producing subtly wrong typography.

**Prevention:** Update the `<link>` URL in ALL 5 HTML pages. Current URL loads `Montserrat:wght@600;700`. Must become `Montserrat:wght@600;700;800`. Easy to miss one file.

### Pitfall 10: Reveal Applied to Above-Fold Elements

**What goes wrong:** Elements visible on page load (above the fold) have `data-reveal`, so they flash from invisible to visible immediately, creating an awkward blink.

**Prevention:** Do NOT add `data-reveal` to any element visible without scrolling. The hero, navigation, page title, and first section should always be immediately visible. Only animate content the user scrolls to.

### Pitfall 11: Hover Effects Inaccessible on Touch Devices

**What goes wrong:** Product card hover effects (image zoom, overlay reveal) are invisible to mobile users who cannot hover.

**Prevention:** Ensure all content is visible and readable without hover interaction. Hover effects should enhance, not gate content. If using a hover overlay to show a "Learn more" label, that label must be visible by default on mobile.

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|---------------|------------|
| Typography + spacing | clamp() values too aggressive | Test resize from 320px to 1400px |
| Typography + spacing | Forgetting to update Google Fonts link in all 5 pages | Grep for the old URL across all HTML files |
| Scroll reveal setup | FOIC if JS fails | noscript fallback styles in every page head |
| Scroll reveal setup | Forgetting defer attribute | Consistent script tag pattern |
| Image overlays | Insufficient contrast with real images | Test with actual metamedical.com.br images |
| Product grid hover | Hover-only content invisible on mobile | Ensure touch devices see all content without hover |
| Cross-page integration | Inconsistent data-reveal placement | Use same pattern on all 5 pages; do not over-animate |

## Sources

- [W3C WCAG 2.1 SC 2.3.3 -- Animation from Interactions](https://www.w3.org/WAI/WCAG21/Understanding/animation-from-interactions.html) -- HIGH confidence
- [prefers-reduced-motion MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion) -- HIGH confidence
- [web.dev -- Cumulative Layout Shift](https://web.dev/articles/cls) -- HIGH confidence
- [Smashing Magazine -- Respecting Users' Motion Preferences](https://www.smashingmagazine.com/2021/10/respecting-users-motion-preferences/) -- MEDIUM confidence
- [Pope Tech -- Accessible Animation](https://blog.pope.tech/2025/12/08/design-accessible-animation-and-movement/) -- MEDIUM confidence
