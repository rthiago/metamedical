# Feature Landscape

**Domain:** Visual polish for medical device corporate website (plain HTML/CSS/vanilla JS)
**Researched:** 2026-02-18

## Table Stakes

Features users expect on a polished medical/corporate site. Missing = site feels amateur or dated.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Scroll reveal animations | Every modern corporate site fades content in on scroll. Static pages feel flat and outdated to healthcare buyers evaluating vendor credibility. | Medium | Use IntersectionObserver + CSS transitions. ~30 lines JS, reusable across all 5 pages. |
| Hero gradient overlay upgrade | Current hero uses flat `rgba(0,0,0,0.55)`. Premium sites use directional gradients with brand-color tinting for depth and mood. | Low | CSS-only change to `.hero::before`. No HTML changes needed. |
| Generous section spacing | Current `--space-2xl: 4rem` padding is adequate but not "breathing". Carrd-style layouts use 6-8rem section padding for premium feel. | Low | Adjust CSS custom properties. Add `--space-3xl: 6rem` and `--space-4xl: 8rem`. |
| Typography scale upgrade | Current h1 at 2.5rem/3rem feels workmanlike. Premium medical sites use 3.5-4.5rem display headings with tighter letter-spacing and lighter weight for elegance. | Low | CSS-only. Adjust font sizes, add `letter-spacing: -0.02em` on headings, use `font-weight: 600` instead of 700 for display sizes. |
| Card styling refinement | Current cards have `border-radius: 4px` and basic shadow. Modern standard is 8-12px radius with layered shadows for depth. | Low | Update `--radius` and `--shadow-card` custom properties. |
| Smooth hover transitions | Already partially present (translateY, box-shadow). Needs consistency: all interactive elements should have unified transition timing and feel. | Low | Audit existing hovers, standardize to consistent easing curve. |
| `prefers-reduced-motion` support | Accessibility requirement (WCAG 2.1 SC 2.3.3). Any scroll animations MUST respect this. Healthcare audience may include users with vestibular disorders. | Low | CSS media query wrapping all animations. Must be implemented alongside any animation work. |

## Differentiators

Features that elevate the site from "functional corporate" to "premium medical brand". Not expected but create competitive advantage when healthcare professionals compare vendors.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Staggered scroll reveals | Cards and value items animate in sequentially (50-100ms delay per item) rather than all at once. Creates visual rhythm and guides the eye. | Medium | CSS `transition-delay` calculated via `data-delay` attributes or CSS `nth-child`. Requires JS for intersection detection + CSS for stagger timing. |
| Hero text entrance animation | Hero h1 and tagline fade-up on page load with slight stagger. First impression sets premium tone. Carrd-style templates almost always animate hero text in. | Low | CSS `@keyframes` with `animation-delay` on `.hero h1` and `.hero .tagline`. No JS needed. |
| Product card image hover zoom | Subtle `transform: scale(1.05)` on product images within cards on hover, with `overflow: hidden` on card. Signals interactivity, adds visual interest. | Low | CSS-only. Add `overflow: hidden` to card, `transform: scale(1.05)` on `img:hover` with transition. |
| Section divider accents | Subtle decorative elements between sections (thin gradient lines, small brand-color accent bars above h2 headings). Breaks visual monotony. | Low | CSS `::before` pseudo-elements on section headings. 2-3px wide accent bar in brand blue. |
| Full-width product showcase sections | Products page currently uses a grid within a container. A premium approach: alternating left-right layout with large product image on one side, text on the other, full-bleed backgrounds. | High | Requires HTML restructure of `produtos.html`. New CSS layout classes. Significant change to product detail structure. |
| Gradient accent on buttons | Add subtle gradient background or gradient border to CTAs instead of flat color. Creates visual weight without clutter. | Low | CSS `background: linear-gradient(...)` on `.btn`. Maintain hover state contrast. |
| Animated counter/stats section | A "numbers" section (years in business, products, certifications) with count-up animation. Common on B2B medical sites for credibility signals. | Medium | Requires new HTML section + JS counter animation with IntersectionObserver trigger. |
| Subtle background patterns | Light dot grid or diagonal line pattern on `section--alt` backgrounds instead of flat #f8f9fa. Adds texture without distraction. | Low | CSS `background-image` with tiny SVG data URI pattern. Opacity 3-5%. |

## Anti-Features

Features to explicitly NOT build. Either inappropriate for a medical device audience, technically risky for this stack, or poor ROI.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Parallax scrolling backgrounds | Causes motion sickness / vestibular issues. Healthcare audience sensitivity. Performance issues on mobile. Feels dated (2015-era trend). | Use fixed hero images with gradient overlays. Subtle fade-in effects provide motion without parallax problems. |
| CSS scroll-driven animations (`animation-timeline: scroll()`) | 78% browser support. Firefox requires flag. Safari 26+ only (released recently). Healthcare professionals often use older browsers / hospital IT-managed machines. | Use IntersectionObserver (98%+ support) for scroll-triggered reveals. Progressive enhancement only if targeting cutting-edge browsers. |
| Auto-playing video backgrounds | Heavy bandwidth. Hospital networks are often restricted. Distracting for clinical audience. Accessibility nightmare. | Static hero images with gradient overlays achieve premium feel without the baggage. |
| Complex JavaScript animation libraries (GSAP, Framer Motion) | Overkill for this scope. Adds 30-100KB dependency for what CSS + minimal JS can achieve. Site is vanilla HTML/CSS with zero JS currently. | IntersectionObserver (~30 lines) + CSS transitions cover all needed animations without dependencies. |
| Animated hamburger menu overhaul | Current CSS-only checkbox hack works well. Replacing with JS-driven animation adds complexity for negligible visual gain. | Keep existing hamburger. Polish the transition timing if needed. |
| Custom font files / variable fonts | Adds complexity to font loading. Google Fonts CDN handles caching, subsetting, and WOFF2 delivery better than self-hosting for this site's scale. | Stick with Google Fonts. Consider upgrading heading font (see Typography section below). |
| Dark mode toggle | Scope creep. Medical/corporate sites rarely need dark mode. Not expected by B2B healthcare audience. Doubles CSS maintenance. | Focus polish budget on light theme quality. |
| Loading skeleton screens | No dynamic content. Static HTML pages load fast. Skeleton screens solve a problem this site does not have. | Ensure fast load times with optimized images and minimal CSS/JS. |

## Feature Dependencies

```
prefers-reduced-motion support ── MUST precede ──> Scroll reveal animations
                                                    |
Hero gradient overlay upgrade ── independent ──     |
                                                    |
Typography scale upgrade ── independent ──          |
                                                    |
Section spacing upgrade ── independent ──           |
                                                    |
Card styling refinement ── should precede ──> Product card image hover zoom
                                                    |
Scroll reveal animations ── should precede ──> Staggered scroll reveals
                                                    |
Hero gradient overlay ── should precede ──> Hero text entrance animation
                                                    |
Section divider accents ── independent ──           |
                                                    |
Full-width product showcase ── independent (but largest scope) ──
```

Key ordering constraints:
1. **Accessibility first:** `prefers-reduced-motion` CSS must exist before any animations land. Non-negotiable.
2. **Foundation before flourish:** Spacing, typography, and card refinements create the baseline. Animations layer on top.
3. **CSS-only before JS-dependent:** Hero entrance, gradient overlay, spacing, and typography need zero JS. Ship these first for immediate visual lift.

## MVP Recommendation

### Phase 1 — CSS-only polish (immediate visual lift, zero JS)

Prioritize these. All are CSS custom property changes or small additions. No HTML changes. No JS.

1. **Typography scale upgrade** — Bump h1 to 3.5rem desktop, add negative letter-spacing, reduce heading weight to 600 at display sizes
2. **Section spacing upgrade** — Add `--space-3xl: 6rem`, apply to `.section` padding
3. **Card styling refinement** — `--radius: 8px`, layered box-shadow, subtle border
4. **Hero gradient overlay** — Replace flat rgba with `linear-gradient(135deg, rgba(0,56,128,0.7), rgba(0,0,0,0.5))` for brand-tinted depth
5. **Section divider accents** — `::before` accent bars on h2 elements
6. **`prefers-reduced-motion` media query** — Foundation for all animation work

### Phase 2 — Animations (JS + CSS, layered on polished base)

7. **Hero text entrance animation** — CSS keyframes, fade-up on load
8. **Scroll reveal animations** — IntersectionObserver + `.reveal` CSS class
9. **Staggered scroll reveals** — `transition-delay` via `nth-child` or data attributes
10. **Product card image hover zoom** — `overflow: hidden` + scale transform

### Defer

- **Full-width product showcase:** High complexity, requires HTML restructure. Consider for a future milestone focused on content/layout redesign.
- **Animated counter/stats section:** Requires new content (what numbers to show?). Defer until company provides data points.
- **Gradient accent on buttons:** Nice-to-have. Current flat blue buttons are clean. Only add if Phase 1 polish makes them feel too plain by comparison.

## Specific Techniques Reference

### Scroll Reveal (IntersectionObserver approach)

```css
/* Base state: invisible, shifted down */
.reveal {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

/* Revealed state */
.reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger children */
.reveal:nth-child(2) { transition-delay: 0.1s; }
.reveal:nth-child(3) { transition-delay: 0.2s; }
.reveal:nth-child(4) { transition-delay: 0.3s; }

/* Accessibility: disable all reveal animations */
@media (prefers-reduced-motion: reduce) {
  .reveal {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

```javascript
// ~20 lines, zero dependencies
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('revealed');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

### Hero Gradient Overlay (brand-tinted)

```css
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  /* Brand blue tint transitioning to dark — premium depth */
  background: linear-gradient(
    135deg,
    rgba(0, 56, 128, 0.75) 0%,
    rgba(0, 0, 0, 0.55) 100%
  );
  z-index: 1;
}
```

### Typography Upgrade

```css
:root {
  /* Display scale for hero/page headings */
  --font-display-size: clamp(2.5rem, 5vw, 4rem);
  --font-display-weight: 600;
  --font-display-tracking: -0.02em;
}

.hero h1 {
  font-size: var(--font-display-size);
  font-weight: var(--font-display-weight);
  letter-spacing: var(--font-display-tracking);
}
```

### Hero Text Entrance

```css
@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero h1 {
  animation: fadeUp 0.8s ease both;
}

.hero .tagline {
  animation: fadeUp 0.8s ease 0.15s both;
}

.hero .btn {
  animation: fadeUp 0.8s ease 0.3s both;
}

@media (prefers-reduced-motion: reduce) {
  .hero h1,
  .hero .tagline,
  .hero .btn {
    animation: none;
  }
}
```

### Product Card Image Zoom

```css
.product-card {
  overflow: hidden; /* already has border-radius */
}

.product-card img {
  transition: transform 0.4s ease;
}

.product-card:hover img {
  transform: scale(1.05);
}
```

## Existing Structure Compatibility

The current codebase is well-structured for visual polish additions:

| Current Structure | Polish Impact | Changes Needed |
|-------------------|--------------|----------------|
| CSS custom properties in `:root` | Easy to adjust spacing, radius, shadows globally | Update property values only |
| `.section` and `.section--alt` classes | Section spacing upgrade is a single rule change | Modify padding value |
| `.card` utility class | Card refinement applies everywhere at once | Update shadow and radius |
| `.hero::before` pseudo-element | Hero overlay upgrade is a background value swap | Change gradient value |
| `.product-card` with `img` children | Image zoom just needs overflow + hover rule | 2 CSS rules |
| No JS files exist | First JS file (scroll-reveal) must be created and linked | Add `<script>` to all 5 HTML files |
| Google Fonts already loaded (Inter + Montserrat) | Typography upgrade works within existing font stack | CSS size/weight/spacing changes only |

## Sources

- [IntersectionObserver scroll animations (freeCodeCamp)](https://www.freecodecamp.org/news/scroll-animations-with-javascript-intersection-observer-api/)
- [CSS scroll-driven animations (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations)
- [CSS animation-timeline scroll() browser support (Can I Use)](https://caniuse.com/mdn-css_properties_animation-timeline_scroll) — 78% support
- [prefers-reduced-motion (CSS-Tricks)](https://css-tricks.com/almanac/rules/m/media/prefers-reduced-motion/)
- [Accessible animation and movement (Pope Tech)](https://blog.pope.tech/2025/12/08/design-accessible-animation-and-movement/)
- [Top Medical Device Websites 2025 (Tribe Agency)](https://tribeagency.org/top-medical-device-websites-2025/)
- [Best Medical Device Website Design Examples (Windmill Strategy)](https://www.windmillstrategy.com/the-best-medical-device-and-life-science-website-design-examples/)
- [Google Font Pairings 2025 (Medium / Bootcamp)](https://medium.com/design-bootcamp/best-google-font-pairings-for-ui-design-in-2025-ba8d006aa03d)
- [CSS gradient overlay techniques (LogRocket)](https://blog.logrocket.com/css-overlay/)
- [Create Scroll Animations with Modern CSS 2026](https://www.moneyhacker.online/blog/create-scroll-animation-with-css)
