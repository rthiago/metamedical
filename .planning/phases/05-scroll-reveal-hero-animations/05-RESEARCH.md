# Phase 5: Scroll Reveal & Hero Animations - Research

**Researched:** 2026-02-18
**Domain:** Vanilla JS scroll animations (IntersectionObserver) + CSS keyframe animations
**Confidence:** HIGH

## Summary

Phase 5 adds two animation systems to a static HTML/CSS site with zero existing JavaScript: (1) scroll-triggered reveal animations using IntersectionObserver, and (2) a hero entrance animation on page load using CSS keyframes with staggered delays.

The approach is straightforward — IntersectionObserver is the standard API for scroll-triggered effects, supported by all modern browsers (97%+ coverage). No libraries needed. The main engineering challenges are the no-JS fallback (content must remain visible without JavaScript) and reduced-motion compliance (all animations suppressed when the OS setting is enabled).

**Primary recommendation:** Use a single `js/animations.js` file that adds a `.js-enabled` class to `<html>`, sets up IntersectionObserver for `.reveal` elements, and triggers hero entrance via CSS classes. CSS handles all visual transitions; JS only toggles classes.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
No locked decisions — user deferred all implementation choices to Claude's discretion.

### Claude's Discretion
- **Scroll reveal style** — fade direction (up, none), travel distance, stagger between sibling elements, threshold for triggering
- **Hero entrance** — which elements animate, stagger order and delays, duration and easing curve
- **Timing & feel** — overall animation speed, delay before first animation, whether to feel snappy or cinematic
- **Edge cases** — behavior on back-navigation, whether reveals re-trigger on scroll up, reduced-motion implementation approach

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope.
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| ANIM-01 | Elements below the fold fade in when scrolled into view (IntersectionObserver) | IntersectionObserver pattern with `.reveal` class and CSS transitions |
| ANIM-02 | Animations respect `prefers-reduced-motion` OS setting (CSS + JS) | CSS media query disables transitions; JS checks matchMedia before observing |
| ANIM-03 | No-JS fallback ensures all content visible without JavaScript | `.reveal` elements visible by default; JS adds `.js-enabled` to `<html>` which hides them |
| HERO-03 | Hero text and CTA fade up on page load with staggered timing | CSS `@keyframes` with `animation-delay` stagger on `.hero h1`, `.hero .tagline`, `.hero .btn` |
</phase_requirements>

## Standard Stack

### Core
| Technology | Version | Purpose | Why Standard |
|------------|---------|---------|--------------|
| IntersectionObserver API | Web API (evergreen) | Detect when elements enter viewport | Native browser API, 97%+ support, zero bundle size |
| CSS @keyframes | CSS3 | Hero entrance animations | Hardware-accelerated, no JS needed for visual part |
| CSS transitions | CSS3 | Scroll reveal fade/slide | Smooth GPU-composited transforms and opacity changes |
| prefers-reduced-motion | CSS media query + JS matchMedia | Accessibility compliance | OS-level setting, standard accessibility requirement |

### Supporting
| Technology | Purpose | When to Use |
|------------|---------|-------------|
| `document.documentElement.classList` | Add `.js-enabled` class | On script load, enables progressive enhancement |
| `Element.classList.add('revealed')` | Trigger reveal transition | When IntersectionObserver fires |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| IntersectionObserver | Scroll event listener | Poor performance, layout thrashing, no threshold support |
| Vanilla JS | AOS library (7KB) | Overkill for simple reveals — adds dependency for no benefit |
| Vanilla JS | GSAP (45KB) | Massive overkill — this site has simple fade-up animations |
| CSS keyframes | JS-driven hero animation | Unnecessary complexity — CSS handles staggered delays natively |

## Architecture Patterns

### Recommended File Structure
```
js/
└── animations.js       # Single file: IO setup + hero trigger + reduced-motion check

css/
└── style.css           # Add reveal classes + hero keyframes + reduced-motion query
```

### Pattern 1: Progressive Enhancement with `.js-enabled`
**What:** JavaScript adds a class to `<html>` immediately on load. CSS uses this class to set initial hidden state for reveal elements.
**When to use:** Always — this is the no-JS fallback strategy.
**Example:**
```javascript
// animations.js — top of file
document.documentElement.classList.add('js-enabled');
```
```css
/* Only hide elements when JS is available */
.js-enabled .reveal {
  opacity: 0;
  transform: translateY(24px);
}

/* Revealed state */
.js-enabled .reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}

/* Without JS: .reveal has no hidden state — content just shows */
```

### Pattern 2: IntersectionObserver with `once: true` Behavior
**What:** Observe elements, add `.revealed` class when they enter viewport, then unobserve.
**When to use:** For scroll reveal — elements should animate in once and stay visible.
**Example:**
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('revealed');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.15 });

document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

### Pattern 3: Hero Entrance with CSS Keyframes
**What:** CSS `@keyframes` with `animation-delay` for staggered hero elements.
**When to use:** Page load hero animation — no scroll detection needed.
**Example:**
```css
@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.js-enabled .hero h1,
.js-enabled .hero .tagline,
.js-enabled .hero .btn {
  opacity: 0;
  animation: fadeUp 0.6s var(--transition-easing) forwards;
}

.js-enabled .hero h1 { animation-delay: 0.1s; }
.js-enabled .hero .tagline { animation-delay: 0.25s; }
.js-enabled .hero .btn { animation-delay: 0.4s; }
```

### Pattern 4: Reduced Motion — Dual Layer (CSS + JS)
**What:** CSS media query removes all animations/transitions. JS also checks and skips observer setup.
**When to use:** Always — accessibility requirement (ANIM-02).
**Example:**
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }

  .js-enabled .reveal {
    opacity: 1;
    transform: none;
  }

  .js-enabled .hero h1,
  .js-enabled .hero .tagline,
  .js-enabled .hero .btn {
    opacity: 1;
    animation: none;
  }
}
```
```javascript
// In animations.js — skip observer if reduced motion
if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
  // Don't observe — content stays visible via CSS override
  return;
}
```

### Anti-Patterns to Avoid
- **Hiding elements in base CSS without JS guard:** Content invisible without JavaScript — violates ANIM-03
- **Using scroll event listeners:** Layout thrashing, poor performance, no threshold support
- **Re-triggering reveals on scroll up:** Annoying on repeat visits; once-revealed elements should stay visible
- **Animating `width`, `height`, `top`, `left`:** Triggers layout recalculation — only animate `transform` and `opacity`
- **Long animation durations (>800ms):** Medical B2B audience expects professional, not flashy

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Viewport detection | Scroll event + getBoundingClientRect | IntersectionObserver | Performance, threshold, browser-optimized |
| Animation timing | setTimeout chains | CSS animation-delay | Declarative, GPU-accelerated, survives tab switches |
| Reduced motion | Manual OS detection | prefers-reduced-motion media query + matchMedia | Standard, automatic, future-proof |

## Common Pitfalls

### Pitfall 1: Flash of Hidden Content (FOHC)
**What goes wrong:** Elements flash visible then disappear when JS loads and adds hidden state.
**Why it happens:** CSS `.reveal { opacity: 0 }` loads before JS, but if JS is slow the observer hasn't fired yet.
**How to avoid:** Gate hidden state behind `.js-enabled` class that JS adds synchronously at parse time. Place `<script>` at end of `<body>` (not `defer`) or use inline script in `<head>`.
**Warning signs:** Content flickers on slow connections.

### Pitfall 2: Elements Never Revealed on Short Pages
**What goes wrong:** IntersectionObserver never fires because elements are already in viewport on load.
**Why it happens:** Observer only fires on intersection *change*. If element is already visible when observed, some browsers don't fire.
**How to avoid:** The observer DOES fire the initial intersection check. Elements already in viewport will get `isIntersecting: true` on first observe. Verified behavior.

### Pitfall 3: Hero Animation Replays on Back Navigation
**What goes wrong:** User navigates away and presses back — hero animates again.
**Why it happens:** bfcache restores page state but CSS animations replay.
**How to avoid:** Accept this behavior — it's brief (0.6s) and not disruptive. Alternatively, use `sessionStorage` flag to skip on revisit, but adds complexity for minimal benefit.

### Pitfall 4: Forgetting `transform` on Revealed State
**What goes wrong:** Elements fade in but remain shifted 24px down.
**Why it happens:** CSS transition on opacity works but `transform: translateY(0)` is missing from `.revealed` class.
**How to avoid:** Always reset both `opacity` and `transform` in the revealed state.

### Pitfall 5: Competing Transitions
**What goes wrong:** Cards already have `transition: transform` for hover. Adding reveal transition conflicts.
**Why it happens:** Both reveal and hover animate `transform`.
**How to avoid:** Use separate transition properties or ensure reveal completes before hover is relevant. Since reveals happen once on scroll and hover is ongoing, the timing naturally separates them. After `.revealed` is added, the element's base `transform` is `translateY(0)` — hover can then adjust from there.

## Code Examples

### Complete animations.js Pattern
```javascript
(function() {
  'use strict';

  // Progressive enhancement: signal JS is available
  document.documentElement.classList.add('js-enabled');

  // Respect reduced motion preference
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    return; // CSS handles visibility; skip all JS animation setup
  }

  // Scroll reveal with IntersectionObserver
  var observer = new IntersectionObserver(function(entries) {
    entries.forEach(function(entry) {
      if (entry.isIntersecting) {
        entry.target.classList.add('revealed');
        observer.unobserve(entry.target);
      }
    });
  }, {
    threshold: 0.15,
    rootMargin: '0px 0px -40px 0px'
  });

  document.querySelectorAll('.reveal').forEach(function(el) {
    observer.observe(el);
  });
})();
```

### CSS Reveal Classes
```css
/* Scroll reveal — hidden state (only with JS) */
.js-enabled .reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.5s var(--transition-easing),
              transform 0.5s var(--transition-easing);
}

.js-enabled .reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}
```

### HTML Integration
```html
<!-- Add .reveal to sections/elements below the fold -->
<section class="section section--alt">
  <div class="container reveal">
    <h2>Nossos Valores</h2>
    ...
  </div>
</section>

<!-- Script at end of body -->
<script src="js/animations.js"></script>
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| jQuery scroll handlers | IntersectionObserver | 2017-2019 | Performance, simplicity |
| AOS/WOW.js libraries | Vanilla IntersectionObserver | 2020+ | Zero dependency, smaller bundle |
| `animation-play-state` hacks | Direct class toggle with IO | 2019+ | Cleaner, more reliable |
| No reduced-motion support | prefers-reduced-motion standard | 2019+ | Accessibility requirement |

## Open Questions

1. **Which elements get `.reveal` class?**
   - What we know: Sections below the fold should animate in
   - What's unclear: Granularity — whole sections vs. individual cards vs. headings
   - Recommendation: Apply to `.container` within each section for clean per-section reveals. Individual card stagger is deferred (ANIM-04 is out of scope)

2. **Script loading strategy**
   - What we know: Need `.js-enabled` added early to prevent FOHC
   - What's unclear: Inline script in `<head>` vs. regular `<script>` at end of `<body>`
   - Recommendation: Regular `<script>` at end of `<body>` — the `.js-enabled` class is added synchronously before any rendering completes. Simpler than inline head script.

## Sources

### Primary (HIGH confidence)
- MDN Web Docs — IntersectionObserver API (standard reference)
- MDN Web Docs — prefers-reduced-motion media query
- MDN Web Docs — CSS @keyframes and animation-delay
- Existing codebase analysis — `css/style.css` (765 lines), 5 HTML pages, existing CSS custom properties and transition system

### Secondary (MEDIUM confidence)
- Web.dev — content-visibility and IntersectionObserver best practices
- Can I Use — IntersectionObserver: 97.4% global support

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - Native browser APIs, no dependencies, well-documented
- Architecture: HIGH - Progressive enhancement pattern is established best practice
- Pitfalls: HIGH - Well-known gotchas with known solutions

**Research date:** 2026-02-18
**Valid until:** 2026-06-18 (stable browser APIs, not fast-moving)
