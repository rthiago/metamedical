# Architecture Patterns

**Domain:** Visual polish for static HTML/CSS site
**Researched:** 2026-02-18

## Recommended Architecture

No architectural changes needed. The site remains 5 static HTML pages + 1 CSS file. The only structural addition is one small JS file.

### Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|---------------|-------------------|
| `css/style.css` | All styling: layout, typography, animations, overlays, reveals | HTML via class names and data attributes |
| `js/reveal.js` (NEW) | Detect elements entering viewport, toggle `.revealed` class | CSS via `.revealed` class; HTML via `data-reveal` attributes |
| HTML pages (5) | Content structure, `data-reveal` attributes on animated elements | CSS + JS |
| Google Fonts CDN | Font delivery | HTML `<link>` tags |

### Data Flow

```
Page Load
  |
  v
HTML parsed --> CSS applied --> [data-reveal] elements hidden (opacity: 0)
  |
  v
JS loaded (defer) --> IntersectionObserver created
  |                        |
  v                        v
Check prefers-reduced-motion    Observe all [data-reveal] elements
  |                                    |
  YES: add .revealed to all,           |
       skip observer                   |
  |                                    v
  NO: observer watches scroll    Element enters viewport
                                       |
                                       v
                                 Add .revealed class
                                       |
                                       v
                                 CSS transition fires (opacity, transform)
                                       |
                                       v
                                 Unobserve element (one-time animation)
```

## Patterns to Follow

### Pattern 1: Data Attribute Driven Animations

**What:** Use `data-reveal` attributes to declare animation type in HTML. CSS reads these for base state. JS toggles class.

**When:** Any element that should animate on scroll.

**Why:** Separation of concerns. HTML declares intent, CSS defines appearance, JS handles detection. Easy to add/remove animations by editing HTML alone.

**Example:**
```html
<!-- HTML: declare animation type -->
<div class="value-card card" data-reveal data-reveal-delay="1">
  <h3>Compromisso</h3>
</div>

<!-- Default data-reveal = fade up. Use data-reveal="left" for slide-left, etc. -->
```

### Pattern 2: Extend Existing Overlay Pattern

**What:** The `.hero::before` overlay pattern is already established. Reuse it via a utility class `.overlay-section`.

**When:** Any full-width section with a background image needing text on top.

**Why:** DRY. One pattern, multiple uses. Consistent visual language across pages.

**Example:**
```css
.overlay-section {
  position: relative;
  background: center/cover no-repeat;
  color: var(--color-white);
}
.overlay-section::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, rgba(0,56,179,0.7), rgba(0,30,80,0.85));
  z-index: 1;
}
.overlay-section > * {
  position: relative;
  z-index: 2;
}
```

### Pattern 3: CSS Custom Properties for Animation Config

**What:** Define animation timing and distances as CSS custom properties in `:root`.

**When:** Always -- enables consistent animation feel and easy global tweaking.

**Example:**
```css
:root {
  --reveal-distance: 30px;
  --reveal-duration: 0.6s;
  --reveal-easing: ease;
}
```

### Pattern 4: Script Loading with `defer`

**What:** Load `reveal.js` with the `defer` attribute so it runs after HTML parsing.

**When:** Always. The script needs DOM elements to exist before querying them.

**Example:**
```html
<script src="js/reveal.js" defer></script>
```

## Anti-Patterns to Avoid

### Anti-Pattern 1: Scroll Event Listeners

**What:** Using `window.addEventListener('scroll', ...)` to detect element visibility.

**Why bad:** Fires on every scroll frame (60+ times/second). Causes layout thrashing if checking `getBoundingClientRect()`. Blocks main thread. Janky scrolling.

**Instead:** Use IntersectionObserver (asynchronous, browser-optimized).

### Anti-Pattern 2: Inline Styles for Animation States

**What:** Setting `element.style.opacity = 1` in JS.

**Why bad:** Mixes concerns. Hard to override. Cannot use CSS transitions smoothly. Cannot respect `prefers-reduced-motion` from CSS alone.

**Instead:** Toggle classes. Let CSS handle all visual states.

### Anti-Pattern 3: Animating Layout Properties

**What:** Animating `width`, `height`, `top`, `left`, `margin`, `padding`.

**Why bad:** Triggers layout recalculation (reflow). Expensive. Causes jank.

**Instead:** Only animate `opacity` and `transform` (compositor-only properties, GPU-accelerated).

### Anti-Pattern 4: Re-animating on Scroll Back

**What:** Removing `.revealed` class when element scrolls out, re-animating when it scrolls back in.

**Why bad:** Distracting, causes motion sickness, feels gimmicky. Medical site should feel professional, not playful.

**Instead:** Animate once. Call `observer.unobserve(entry.target)` after reveal.

## Scalability Considerations

Not a concern for this project. The site is 5 static pages with ~15-20 animated elements total. IntersectionObserver handles this effortlessly. No performance bottlenecks possible at this scale.

If the site ever grows to 50+ pages or dynamic content: the same architecture scales fine. IntersectionObserver can watch hundreds of elements without performance issues.

## Sources

- [IntersectionObserver API MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) -- HIGH confidence
- [CSS-Tricks: Scroll-Triggered Animation with Vanilla JS](https://css-tricks.com/scroll-triggered-animation-vanilla-javascript/) -- MEDIUM confidence
- [web.dev: Rendering Performance](https://web.dev/articles/rendering-performance) -- HIGH confidence (compositor-only properties)
