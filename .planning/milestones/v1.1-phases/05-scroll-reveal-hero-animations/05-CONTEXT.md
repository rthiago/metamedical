# Phase 5: Scroll Reveal & Hero Animations - Context

**Gathered:** 2026-02-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Content below the fold animates into view on scroll using IntersectionObserver. Hero text and CTA animate on page load with staggered timing. All animations respect prefers-reduced-motion and degrade gracefully with JS disabled.

</domain>

<decisions>
## Implementation Decisions

### Claude's Discretion

User deferred all implementation decisions to Claude. The following areas are open for best-judgment choices during planning and execution:

- **Scroll reveal style** — fade direction (up, none), travel distance, stagger between sibling elements, threshold for triggering
- **Hero entrance** — which elements animate, stagger order and delays, duration and easing curve
- **Timing & feel** — overall animation speed, delay before first animation, whether to feel snappy or cinematic
- **Edge cases** — behavior on back-navigation, whether reveals re-trigger on scroll up, reduced-motion implementation approach

</decisions>

<specifics>
## Specific Ideas

No specific requirements — open to standard approaches.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 05-scroll-reveal-hero-animations*
*Context gathered: 2026-02-18*
