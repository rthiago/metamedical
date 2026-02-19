# Phase 6: Image Overlay Sections - Context

**Gathered:** 2026-02-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Hero section upgrades from flat dark overlay to a branded blue-to-dark gradient overlay. A reusable overlay-section CSS pattern is created and applied to at least one additional section with a full-width background image and readable text.

</domain>

<decisions>
## Implementation Decisions

### Claude's Discretion

User deferred all implementation decisions to Claude. The following areas are open for best-judgment choices during planning and execution:

- **Hero gradient overlay** -- direction (top-to-bottom, left-to-right, diagonal), color stops (brand blue to dark), opacity levels for text readability
- **Additional overlay section** -- which section gets the overlay treatment, what background image to use, how to keep text readable at all screen sizes
- **Reusable overlay-section pattern** -- CSS class naming, how the pattern is structured for reuse, whether it uses CSS custom properties for theming
- **Text contrast** -- minimum contrast ratio approach, whether to add text-shadow or rely purely on overlay opacity
- **Responsive behavior** -- how overlays adapt on mobile (same gradient vs adjusted), background-position for different viewports

</decisions>

<specifics>
## Specific Ideas

No specific requirements -- open to standard approaches.

</specifics>

<deferred>
## Deferred Ideas

None -- discussion stayed within phase scope.

</deferred>

---

*Phase: 06-image-overlay-sections*
*Context gathered: 2026-02-18*
