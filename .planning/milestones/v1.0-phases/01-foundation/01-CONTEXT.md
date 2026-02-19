# Phase 1: Foundation - Context

**Gathered:** 2026-02-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Deployable site shell with consistent header (logo + nav), footer (CNPJ, address, RE 8365-SP), and design system (colors, typography, spacing). Five page stubs (Home, Empresa, Produtos, Localização, Contato) share the shell. Plain HTML/CSS only, no build step. Portuguese only. Mobile hamburger menu required.

</domain>

<decisions>
## Implementation Decisions

### Claude's Discretion
All implementation areas were deferred to Claude's judgment:

- **Visual identity** — Color palette, mood/tone. Professional medical device aesthetic. Choose colors, gradients, and overall visual feel
- **Navigation & header** — Logo placement, link order, hamburger menu behavior on mobile, sticky vs static header
- **Page layout template** — Max-width, content areas, overall page structure for the shared shell
- **Typography & spacing** — Google Fonts selection, heading hierarchy, spacing rhythm, density

</decisions>

<specifics>
## Specific Ideas

- Keep the current MetaMedical logo — do not redesign or replace it
- Logo file will need to be sourced/provided by user (not currently in the repo)

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 01-foundation*
*Context gathered: 2026-02-18*
