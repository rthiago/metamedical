# Roadmap: MetaMedical Website Redesign

## Milestones

- ✅ **v1.0 MVP** — Phases 1-3 (shipped 2026-02-19)
- 🚧 **v1.1 Visual Polish** — Phases 4-6 (in progress)

## Phases

<details>
<summary>✅ v1.0 MVP (Phases 1-3) — SHIPPED 2026-02-19</summary>

- [x] Phase 1: Foundation (1/1 plan) — completed 2026-02-18
- [x] Phase 2: Content Pages (2/2 plans) — completed 2026-02-18
- [x] Phase 3: Polish & Deploy (1/1 plan) — completed 2026-02-18

</details>

### 🚧 v1.1 Visual Polish

**Milestone Goal:** Comprehensive visual refresh -- premium feel with bold typography, scroll animations, image overlays, and refined spacing.

- [x] **Phase 4: Typography & Spacing Foundation** - Fluid sizing, generous spacing, card refinements, and unified transitions — completed 2026-02-18
- [x] **Phase 5: Scroll Reveal & Hero Animations** - IntersectionObserver reveals, hero entrance, and accessibility fallbacks — completed 2026-02-18
- [ ] **Phase 6: Image Overlay Sections** - Hero gradient upgrade and reusable overlay-section pattern

## Phase Details

### Phase 4: Typography & Spacing Foundation
**Goal**: Every page feels premium through refined typography, generous spacing, polished cards, and consistent interaction timing
**Depends on**: Phase 3 (v1.0 complete)
**Requirements**: TYPO-01, TYPO-02, TYPO-03, TYPO-04, HOVR-01
**Success Criteria** (what must be TRUE):
  1. Headings scale smoothly from mobile to desktop without any abrupt size jumps
  2. Sections have visibly more breathing room than v1.0 -- content never feels cramped
  3. Cards show rounded corners and subtle layered shadows that lift them off the page
  4. Every H2 heading displays a small colored accent bar above it
  5. All buttons, links, and cards animate on hover with the same speed and easing
**Plans**: 2 plans

Plans:
- [x] 04-01-PLAN.md — Fluid typography with clamp() and generous section spacing
- [x] 04-02-PLAN.md — Card refinement, H2 accent bars, and unified transitions

### Phase 5: Scroll Reveal & Hero Animations
**Goal**: Content below the fold animates into view on scroll, the hero entrance feels alive, and all animations degrade gracefully
**Depends on**: Phase 4
**Requirements**: ANIM-01, ANIM-02, ANIM-03, HERO-03
**Success Criteria** (what must be TRUE):
  1. Elements below the fold fade in as the user scrolls down the page
  2. Hero text and CTA button fade up with staggered timing on page load
  3. With OS reduced-motion enabled, all animations are suppressed and content appears instantly
  4. With JavaScript disabled, all content is visible -- nothing is hidden or invisible
**Plans**: 1 plan

Plans:
- [x] 05-01-PLAN.md — Scroll reveal animations, hero entrance, reduced-motion and no-JS fallbacks

### Phase 6: Image Overlay Sections
**Goal**: Hero and feature sections use photographic backgrounds with branded overlays for visual depth
**Depends on**: Phase 4
**Requirements**: HERO-01, HERO-02
**Success Criteria** (what must be TRUE):
  1. Hero section displays a blue-to-dark gradient overlay instead of the previous flat dark overlay
  2. At least one additional section uses a full-width background image with text overlay, readable at all screen sizes
**Plans**: 1 plan

Plans:
- [ ] 06-01-PLAN.md — Hero gradient upgrade, reusable overlay-section pattern, Produtos CTA overlay

## Progress

**Execution Order:** 4 → 5 → 6

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Foundation | v1.0 | 1/1 | Complete | 2026-02-18 |
| 2. Content Pages | v1.0 | 2/2 | Complete | 2026-02-18 |
| 3. Polish & Deploy | v1.0 | 1/1 | Complete | 2026-02-18 |
| 4. Typography & Spacing Foundation | v1.1 | 2/2 | Complete | 2026-02-18 |
| 5. Scroll Reveal & Hero Animations | v1.1 | 1/1 | Complete | 2026-02-18 |
| 6. Image Overlay Sections | v1.1 | 0/1 | Planned | - |
