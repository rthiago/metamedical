# Phase 6: Image Overlay Sections - Research

**Researched:** 2026-02-18
**Domain:** CSS gradient overlays on background images
**Confidence:** HIGH

## Summary

Phase 6 replaces the hero's flat dark overlay (`rgba(0,0,0,0.55)`) with a branded blue-to-dark gradient, and creates a reusable `.overlay-section` CSS pattern for full-width image sections with text. This is pure CSS work -- no new libraries, no JavaScript changes.

The brand primary color is `#0056b3` (--color-primary). The gradient overlay should transition from a semi-transparent brand blue at the top to a dark near-black at the bottom, maintaining text readability. The reusable pattern uses the same `::before` pseudo-element technique already established in the hero.

**Primary recommendation:** Modify `.hero::before` to use a linear-gradient instead of solid rgba. Create `.overlay-section` class with configurable gradient via CSS custom properties. Apply to one additional section (the Empresa page "Nossos Valores" or a new CTA-style section on the home page).

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
No locked decisions -- user deferred all implementation choices to Claude's discretion.

### Claude's Discretion
- Hero gradient overlay -- direction, color stops, opacity levels
- Additional overlay section -- which section, what image, readability approach
- Reusable overlay-section pattern -- CSS class naming, structure, custom properties
- Text contrast -- contrast ratio approach, text-shadow vs overlay opacity
- Responsive behavior -- overlay adaptation on mobile

### Deferred Ideas (OUT OF SCOPE)
None.
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| HERO-01 | Hero section uses brand-tinted gradient overlay (blue-to-dark) instead of flat black | Replace `rgba(0,0,0,0.55)` in `.hero::before` with `linear-gradient(to bottom, rgba(0,86,179,0.7), rgba(0,0,0,0.85))` |
| HERO-02 | Reusable overlay-section CSS pattern for full-width image sections with text | Create `.overlay-section` class with `::before` gradient, `background: url() center/cover`, z-index layering |
</phase_requirements>

## Standard Stack

### Core
| Technology | Version | Purpose | Why Standard |
|------------|---------|---------|--------------|
| CSS linear-gradient | CSS3 | Gradient overlays on ::before pseudo-elements | Native, zero bundle cost, GPU-composited |
| CSS custom properties | CSS3 | Configurable gradient colors per instance | Already used extensively in the design system |
| background: url() center/cover | CSS3 | Full-width background images | Standard responsive image technique |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| ::before overlay | background-blend-mode | Less browser support, less control over gradient shape |
| linear-gradient | radial-gradient | Linear is more natural for full-section overlays |
| CSS custom properties | Hardcoded values | Custom properties allow reuse with different colors |

## Architecture Patterns

### Pattern 1: Gradient Overlay via ::before
**What:** Replace solid color with linear-gradient in the existing `::before` pseudo-element.
**When to use:** Hero section upgrade (HERO-01).
**Example:**
```css
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  /* Old: background: rgba(0, 0, 0, 0.55); */
  background: linear-gradient(
    to bottom,
    rgba(0, 86, 179, 0.7) 0%,
    rgba(0, 40, 80, 0.8) 50%,
    rgba(0, 0, 0, 0.85) 100%
  );
  z-index: 1;
}
```

### Pattern 2: Reusable .overlay-section Class
**What:** A utility class that turns any section into a full-width image section with gradient overlay.
**When to use:** Any section that needs a photographic background with text overlay (HERO-02).
**Example:**
```css
.overlay-section {
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  color: var(--color-white);
  overflow: hidden;
}

.overlay-section::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    var(--overlay-from, rgba(0, 86, 179, 0.7)),
    var(--overlay-to, rgba(0, 0, 0, 0.85))
  );
  z-index: 1;
}

.overlay-section > * {
  position: relative;
  z-index: 2;
}
```

### Pattern 3: Per-Instance Background via Inline Style
**What:** Set the background image per element via inline `style` attribute in HTML, keep overlay in CSS.
**When to use:** When different sections need different background images.
**Example:**
```html
<section class="overlay-section" style="background-image: url('https://metamedical.com.br/pic/medical-slide-1.jpg')">
  <div class="container">
    <h2>Section Title</h2>
    <p>Content here...</p>
  </div>
</section>
```

### Anti-Patterns to Avoid
- **Hardcoding background-image in CSS:** Makes it impossible to reuse the pattern with different images
- **Using `background-blend-mode` instead of `::before`:** Less control, inconsistent browser rendering
- **Forgetting z-index layering:** Text must be above overlay (z-index: 2), overlay above image (z-index: 1)
- **Too-transparent overlays:** Text becomes unreadable on bright image areas -- minimum 0.7 opacity on dark end

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Gradient overlay | JavaScript-based overlay | CSS ::before + linear-gradient | Zero JS, GPU-composited, simpler |
| Image responsive sizing | JS-based image sizing | background-size: cover | Native, handles all viewport sizes |
| Per-section customization | Separate CSS class per section | CSS custom properties (--overlay-from, --overlay-to) | One pattern, many instances |

## Common Pitfalls

### Pitfall 1: Text Unreadable on Light Image Areas
**What goes wrong:** Gradient overlay is too transparent, text becomes hard to read over bright portions of background image.
**Why it happens:** Gradient opacity tuned for one image doesn't work for another.
**How to avoid:** Use minimum 0.7 opacity on the dark end of gradient. Test with the actual metamedical.com.br product images.
**Warning signs:** Squinting to read text, WCAG contrast ratio below 4.5:1.

### Pitfall 2: H2 Accent Bar Invisible on Dark Background
**What goes wrong:** The `h2::before` accent bar (brand blue on white) becomes invisible on a blue overlay background.
**Why it happens:** Phase 4 added blue accent bars to all H2s. On an overlay section, blue-on-blue is invisible.
**How to avoid:** Override `h2::before` inside `.overlay-section` to use white or remove it. Example:
```css
.overlay-section h2::before {
  background-color: var(--color-white);
}
```

### Pitfall 3: Hero Animation Conflict
**What goes wrong:** Phase 5's hero entrance animation uses `.js-enabled .hero h1` etc. Changing overlay class names could break selectors.
**Why it happens:** CSS specificity or class name changes.
**How to avoid:** Keep `.hero` class on the hero section. The `.overlay-section` class is additive, not replacement. Hero gets both: `class="hero overlay-section"` or hero keeps its own `::before` override.

### Pitfall 4: Content Container Missing z-index
**What goes wrong:** Text appears behind the overlay.
**Why it happens:** `.overlay-section > *` needs `position: relative; z-index: 2` to sit above `::before`.
**How to avoid:** The pattern includes z-index layering. Test that text is visible and clickable.

## Code Examples

### Complete Hero Gradient Upgrade
```css
/* Replace the existing .hero::before */
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(0, 86, 179, 0.7) 0%,
    rgba(0, 40, 80, 0.8) 50%,
    rgba(0, 0, 0, 0.85) 100%
  );
  z-index: 1;
}
```

### Reusable Overlay Section
```css
.overlay-section {
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  color: var(--color-white);
  overflow: hidden;
}

.overlay-section::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    var(--overlay-from, rgba(0, 86, 179, 0.7)),
    var(--overlay-to, rgba(0, 0, 0, 0.85))
  );
  z-index: 1;
}

.overlay-section > * {
  position: relative;
  z-index: 2;
}

/* Fix h2 accent bar visibility on overlay backgrounds */
.overlay-section h2 {
  color: var(--color-white);
}

.overlay-section h2::before {
  background-color: var(--color-white);
}

/* Fix link colors in overlay sections */
.overlay-section a {
  color: var(--color-white);
}

.overlay-section .btn {
  color: var(--color-white);
}
```

### Which Section Gets the Overlay Treatment
Best candidate for the additional overlay section: the **Empresa page "Nossos Valores" section** or a **new CTA section on index.html**. The Produtos page "Solicite Nosso Catalogo" section is also a good candidate -- it's already a CTA-style section with `section--alt` background.

Recommendation: Apply to the **Produtos page catalog CTA section** -- it's a natural fit for a photographic background (medical equipment imagery) with a call-to-action overlay.

## Open Questions

1. **Which background image for the additional overlay section?**
   - What we know: metamedical.com.br has product images at `/pic/` path
   - Recommendation: Use `medical-slide-1.jpg` (already used in hero) or find another medical image from the existing CDN

## Sources

### Primary (HIGH confidence)
- Existing codebase analysis -- css/style.css hero section (`.hero::before` at line 303)
- MDN Web Docs -- CSS linear-gradient, ::before pseudo-element, CSS custom properties
- Existing design system -- CSS custom properties in :root (--color-primary: #0056b3)

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - Pure CSS, well-documented, already used in codebase
- Architecture: HIGH - Extending existing ::before pattern
- Pitfalls: HIGH - Known gotchas with established solutions

**Research date:** 2026-02-18
**Valid until:** 2026-06-18 (stable CSS, not fast-moving)
