# Phase 4: Typography & Spacing Foundation - Research

**Researched:** 2026-02-18
**Domain:** CSS fluid typography, spacing systems, card refinement, transition unification
**Confidence:** HIGH

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
None — user deferred all visual decisions to Claude.

### Claude's Discretion
- **Fluid typography** — clamp ranges, scale ratio, font weight choices
- **Section spacing** — gap sizes, vertical rhythm, breathing room between sections
- **Card polish** — shadow depth/layers, border radius, internal padding
- **H2 accent bars** — color (brand palette), width, thickness, position relative to heading
- **Hover & transition timing** — duration, easing curve, which elements get hover effects
- **Consistency** — ensuring all interactive elements share the same transition speed and easing

### Deferred Ideas (OUT OF SCOPE)
None.
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| TYPO-01 | All headings use fluid `clamp()` sizing that scales smoothly from mobile to desktop | Fluid typography section: clamp() formulas with recommended ranges |
| TYPO-02 | Section padding uses generous spacing (6rem+) for premium breathing room | Spacing system section: new custom properties with 6rem+ values |
| TYPO-03 | Cards use refined styling with 8px border-radius and layered box-shadows | Card refinement section: layered shadow technique and radius update |
| TYPO-04 | H2 headings display a small brand-color accent bar above them | H2 accent bar section: ::before pseudo-element pattern |
| HOVR-01 | All interactive elements use unified transition timing and easing | Transition unification section: single timing variable strategy |
</phase_requirements>

## Summary

Phase 4 is entirely CSS-based — no JavaScript, no new dependencies, no build tools. The current codebase uses a single `style.css` with CSS custom properties, mobile-first breakpoints at 768px and 1024px, and static `rem` font sizes. The upgrade path is straightforward: replace fixed heading sizes with `clamp()` functions, increase section padding custom properties, refine card shadows and radii, add `::before` accent bars on H2 elements, and unify all transition declarations to a single timing/easing variable.

The site uses Google Fonts (Inter for body, Montserrat for headings) loaded via `<link>` tags. Current font weights loaded are Inter 400/500/600 and Montserrat 600/700. The existing `--transition: 0.3s ease` variable is already used across most components but not consistently — some elements inline their own timing. Unification means auditing every `transition` property and ensuring they all reference the same custom property.

**Primary recommendation:** Implement all changes as CSS custom property updates and new rules in `style.css` — no structural HTML changes needed, no new files required.

## Standard Stack

### Core
| Technology | Version | Purpose | Why Standard |
|-----------|---------|---------|--------------|
| CSS `clamp()` | CSS3 (97%+ browser support) | Fluid font sizing without media queries | Native, zero-dependency, supported in all modern browsers |
| CSS Custom Properties | CSS3 | Design token system | Already used in codebase, enables single-source updates |
| CSS `::before` pseudo-element | CSS2+ | H2 accent bars without extra HTML | No markup changes needed, pure presentation layer |

### Supporting
| Technology | Purpose | When to Use |
|-----------|---------|-------------|
| Montserrat 800 weight | Bolder headings for premium feel | Add to Google Fonts import if heading weight upgrade desired |
| `cubic-bezier()` easing | More refined hover animations | Optional upgrade from `ease` — subtle but noticeable |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| `clamp()` | Media query breakpoints | More code, abrupt jumps between sizes — clamp is smoother |
| CSS custom properties for spacing | Utility classes (Tailwind-style) | Overkill for 5-page static site, custom properties already in use |
| `::before` for accent bars | `<span>` or `<hr>` elements | Requires HTML changes across all pages, semantic pollution |

## Architecture Patterns

### Pattern 1: Fluid Type Scale with clamp()

**What:** Replace fixed `font-size` values with `clamp(min, preferred, max)` where the preferred value uses viewport units for smooth scaling.

**Formula:** `clamp(min-rem, calc(base + vw-scale), max-rem)`

**Recommended scale for this project:**

```css
/* Fluid type scale — mobile-first, scales smoothly to desktop */
:root {
  --text-base: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);          /* 16px → 18px */
  --text-lg: clamp(1.125rem, 1rem + 0.5vw, 1.375rem);            /* 18px → 22px */
  --text-xl: clamp(1.25rem, 1.1rem + 0.75vw, 1.75rem);           /* 20px → 28px */
  --text-2xl: clamp(1.5rem, 1.2rem + 1.5vw, 2.25rem);            /* 24px → 36px */
  --text-3xl: clamp(1.875rem, 1.5rem + 1.875vw, 2.75rem);        /* 30px → 44px */
  --text-hero: clamp(2.25rem, 1.75rem + 2.5vw, 3.5rem);          /* 36px → 56px */
}
```

**Why these ranges:**
- Mobile minimum is readable (never below 16px body)
- Desktop maximum prevents oversized headings on ultrawide screens
- The `vw` component creates smooth interpolation between breakpoints
- Hero text stays impactful on mobile (2.25rem = 36px) while commanding on desktop (3.5rem = 56px)

**When to use:** Every heading and body text declaration. Remove all fixed `font-size` from heading rules and mobile/tablet media queries.

### Pattern 2: Generous Spacing System

**What:** Extend the existing spacing scale with larger values for section padding.

```css
:root {
  /* Keep existing small values */
  --space-xs: 0.5rem;    /* 8px */
  --space-sm: 1rem;      /* 16px */
  --space-md: 1.5rem;    /* 24px */
  --space-lg: 2rem;      /* 32px */
  --space-xl: 3rem;      /* 48px */
  --space-2xl: 4rem;     /* 64px — CURRENT section padding */

  /* NEW: Premium spacing */
  --space-3xl: 6rem;     /* 96px — new section padding */
  --space-4xl: 8rem;     /* 128px — hero/major sections */
}
```

**Section padding upgrade:** Change `.section { padding: var(--space-2xl) 0; }` to `padding: var(--space-3xl) 0;` — immediately adds 32px more breathing room per section (top + bottom).

**On mobile**, section padding should reduce slightly to avoid excessive scrolling:
```css
.section {
  padding: clamp(3rem, 2.5rem + 2.5vw, 6rem) 0;
}
```

### Pattern 3: Layered Box Shadows for Cards

**What:** Replace single-layer `box-shadow` with multi-layer shadows for realistic depth.

```css
:root {
  --radius-card: 8px;   /* upgrade from 4px */

  /* Layered shadows — ambient + direct light */
  --shadow-card:
    0 1px 2px rgba(0, 0, 0, 0.04),
    0 4px 12px rgba(0, 0, 0, 0.06);

  --shadow-card-hover:
    0 2px 4px rgba(0, 0, 0, 0.04),
    0 8px 24px rgba(0, 0, 0, 0.1),
    0 16px 48px rgba(0, 0, 0, 0.04);
}
```

**Why layered:**
- First layer = tight, sharp ambient occlusion (object sits on surface)
- Second layer = main directional shadow (visual depth)
- Third layer (hover) = wide, soft diffuse glow (elevated state)
- Looks significantly more premium than a single `0 2px 8px rgba(0,0,0,0.1)`

### Pattern 4: H2 Accent Bar via ::before

**What:** Small colored bar above H2 headings using a pseudo-element. No HTML changes.

```css
h2 {
  position: relative;
  padding-top: var(--space-sm);
}

h2::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 3px;
  background-color: var(--color-primary);
  border-radius: 2px;
}

/* Centered variant for centered sections */
.hero h2::before,
.section--alt h2::before {
  left: 50%;
  transform: translateX(-50%);
}
```

**Design choices:**
- Width: 40px — visible but not overwhelming
- Height: 3px — crisp on all DPIs
- Color: `--color-primary` (#0056b3) — brand blue, high contrast
- Position: above heading, not below — draws eye downward into content

### Pattern 5: Unified Transition System

**What:** Single transition variable used everywhere, with an optional fast variant.

```css
:root {
  --transition-duration: 0.25s;
  --transition-easing: cubic-bezier(0.4, 0, 0.2, 1);  /* Material "standard" curve */
  --transition: var(--transition-duration) var(--transition-easing);
  --transition-fast: 0.15s var(--transition-easing);
}
```

**Why `cubic-bezier(0.4, 0, 0.2, 1)` over `ease`:**
- `ease` = `cubic-bezier(0.25, 0.1, 0.25, 1.0)` — starts slow, ends abruptly
- Material standard curve — quick start, smooth deceleration — feels more responsive
- 0.25s slightly faster than current 0.3s — snappier, more premium

**Elements that need transition audit:**
- Links (`a`) — color
- Buttons (`.btn`) — background-color, box-shadow, transform
- Cards (`.card`, `.product-card`, `.value-card`, `.contact-card`) — transform, box-shadow
- Nav links — color, border-color
- Empresa value items — background-color, border-color
- Footer links — color

### Anti-Patterns to Avoid

- **Mixing `px`, `em`, and `rem` for font sizes:** Use `clamp()` with `rem` exclusively. The codebase already uses `rem` — maintain this.
- **Over-large clamp ranges:** Don't let hero text exceed 4rem on desktop for a medical B2B site — it would feel casual, not professional.
- **Transition on `all`:** Never use `transition: all 0.3s ease`. Always list specific properties — prevents layout thrashing and unexpected animations.
- **accent bar on every heading:** Only H2 gets the accent bar. H1 (hero only) and H3 (card titles) should not — keeps it special.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Fluid font sizing | Custom JS resize handlers | CSS `clamp()` | Browser-native, zero JS, better performance |
| Spacing consistency | Inline padding values | CSS custom properties | Already in use, single source of truth |
| Shadow depth | Hardcoded `box-shadow` per element | Custom property `--shadow-card` | Change once, applies everywhere |
| Accent bar positioning | JavaScript DOM manipulation | CSS `::before` | Pure CSS, no runtime cost, no flash |

## Common Pitfalls

### Pitfall 1: Clamp() with wrong vw scale
**What goes wrong:** Text is either barely different between mobile/desktop, or jumps too aggressively.
**Why it happens:** The `vw` coefficient is guessed instead of calculated.
**How to avoid:** Use the formula: `preferred = min + (max - min) * ((100vw - 320px) / (1200px - 320px))`. For a 320-1200px range, `1vw ≈ 0.114 of the difference`.
**Warning signs:** Test at 320px and 1200px viewport — sizes should exactly match min and max.

### Pitfall 2: Section spacing too aggressive on mobile
**What goes wrong:** 6rem (96px) padding on a 375px phone makes content feel lost in whitespace.
**How to avoid:** Use fluid spacing: `clamp(3rem, 2.5rem + 2.5vw, 6rem)` — 3rem on mobile, 6rem on desktop.
**Warning signs:** Scroll length doubles on mobile compared to v1.0.

### Pitfall 3: Layered shadows on low-contrast backgrounds
**What goes wrong:** Cards on `--color-bg-alt` (#f8f9fa) look fine, but cards on white backgrounds lose shadow visibility.
**How to avoid:** Test cards on both white and alt backgrounds. The recommended shadow values account for both.

### Pitfall 4: Accent bar breaking centered headings
**What goes wrong:** `left: 0` accent bar looks wrong on centered text sections.
**How to avoid:** Use centered variant for sections with `text-align: center`. The pattern above handles this with `.section--alt h2::before`.

### Pitfall 5: Transition not applied to new shadow property
**What goes wrong:** Card hover shadows "snap" instead of animating.
**Why it happens:** `transition: transform var(--transition)` doesn't include `box-shadow`.
**How to avoid:** Always include `box-shadow` in card transition declarations.

## Code Examples

### Complete Fluid Typography Implementation

```css
/* Custom properties — add to :root */
:root {
  --text-base: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);
  --text-lg: clamp(1.125rem, 1rem + 0.5vw, 1.375rem);
  --text-xl: clamp(1.25rem, 1.1rem + 0.75vw, 1.75rem);
  --text-2xl: clamp(1.5rem, 1.2rem + 1.5vw, 2.25rem);
  --text-3xl: clamp(1.875rem, 1.5rem + 1.875vw, 2.75rem);
  --text-hero: clamp(2.25rem, 1.75rem + 2.5vw, 3.5rem);
}

/* Apply to elements */
body { font-size: var(--text-base); }
p { font-size: var(--text-base); }
h3 { font-size: var(--text-xl); }
h2 { font-size: var(--text-2xl); }
h1 { font-size: var(--text-3xl); }
.hero h1 { font-size: var(--text-hero); }
.tagline { font-size: var(--text-lg); }
```

### Complete Card Refinement

```css
:root {
  --radius-card: 8px;
  --shadow-card: 0 1px 2px rgba(0,0,0,0.04), 0 4px 12px rgba(0,0,0,0.06);
  --shadow-card-hover: 0 2px 4px rgba(0,0,0,0.04), 0 8px 24px rgba(0,0,0,0.1), 0 16px 48px rgba(0,0,0,0.04);
}

.card {
  border-radius: var(--radius-card);
  box-shadow: var(--shadow-card);
  transition: transform var(--transition), box-shadow var(--transition);
}

.card:hover {
  box-shadow: var(--shadow-card-hover);
}
```

### Complete Accent Bar

```css
h2 {
  position: relative;
  padding-top: 1rem;
}

h2::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 3px;
  background-color: var(--color-primary);
  border-radius: 2px;
}
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Media query font sizes | `clamp()` fluid sizing | 2020+ (97%+ support) | Eliminates typography breakpoint code |
| Single box-shadow | Layered multi-shadow | Design trend 2019+ | More realistic depth perception |
| `ease` timing function | `cubic-bezier(0.4, 0, 0.2, 1)` | Material Design popularized | Snappier, more responsive feel |
| `border-bottom` accent | `::before` pseudo-element | Always available | More flexible positioning |

## Open Questions

1. **Montserrat 800 weight for headings?**
   - Current: 700 (Bold) loaded via Google Fonts
   - Option: Add 800 (ExtraBold) for more impactful headings
   - Recommendation: Start with 700, add 800 only if headings feel weak after clamp() upgrade
   - Note from STATE.md: "Verify Montserrat 800 weight renders well on low-DPI screens" — test if added

2. **Footer heading accent bars?**
   - Footer `h3` elements are white on dark background
   - Accent bar would need lighter color (white or accent color)
   - Recommendation: Skip accent bars in footer — keep them H2-only for clear visual hierarchy

## Sources

### Primary (HIGH confidence)
- MDN Web Docs — CSS `clamp()` function specification and browser support
- MDN Web Docs — CSS Custom Properties
- MDN Web Docs — `::before` pseudo-element
- MDN Web Docs — `cubic-bezier()` timing functions
- Google Fonts — Montserrat available weights (100-900)

### Secondary (MEDIUM confidence)
- Material Design — Standard easing curve `cubic-bezier(0.4, 0, 0.2, 1)` specification
- Can I Use — `clamp()` at 97.2% global support (Feb 2026)

## Metadata

**Confidence breakdown:**
- Fluid typography: HIGH — `clamp()` is well-established CSS, no edge cases for this use
- Spacing system: HIGH — extending existing custom properties, no risk
- Card refinement: HIGH — pure CSS shadow/radius changes
- Accent bars: HIGH — `::before` pseudo-elements, widely supported
- Transition unification: HIGH — auditing existing code, replacing values

**Research date:** 2026-02-18
**Valid until:** 2026-06-18 (stable CSS features, long shelf life)
