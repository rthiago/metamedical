# Requirements: MetaMedical Visual Polish

**Defined:** 2026-02-18
**Core Value:** Visitors immediately understand what MetaMedical does and can find product information and contact details with zero friction.

## v1.1 Requirements

Requirements for visual polish milestone. Each maps to roadmap phases.

### Typography & Spacing

- [x] **TYPO-01**: All headings use fluid `clamp()` sizing that scales smoothly from mobile to desktop
- [x] **TYPO-02**: Section padding uses generous spacing (6rem+) for premium breathing room
- [x] **TYPO-03**: Cards use refined styling with 8px border-radius and layered box-shadows
- [x] **TYPO-04**: H2 headings display a small brand-color accent bar above them

### Hero & Overlays

- [ ] **HERO-01**: Hero section uses brand-tinted gradient overlay (blue-to-dark) instead of flat black
- [ ] **HERO-02**: Reusable overlay-section CSS pattern for full-width image sections with text
- [ ] **HERO-03**: Hero text and CTA fade up on page load with staggered timing

### Scroll Animations

- [ ] **ANIM-01**: Elements below the fold fade in when scrolled into view (IntersectionObserver)
- [ ] **ANIM-02**: Animations respect `prefers-reduced-motion` OS setting (CSS + JS)
- [ ] **ANIM-03**: No-JS fallback ensures all content visible without JavaScript

### Hover & Transitions

- [x] **HOVR-01**: All interactive elements use unified transition timing and easing

## Future Requirements

### Deferred from v1.1

- **ANIM-04**: Staggered scroll reveals with sequential timing on grid items
- **CARD-01**: Product card images zoom subtly on hover

## Out of Scope

| Feature | Reason |
|---------|--------|
| Parallax scrolling | Motion sickness risk, dated feel, poor mobile performance |
| CSS scroll-driven animations | Firefox lacks support (behind flag only), 78% browser coverage |
| Animation libraries (AOS, GSAP) | Overkill — vanilla JS IntersectionObserver suffices |
| Dark mode | Scope creep, not expected for medical B2B audience |
| Video backgrounds | Heavy bandwidth, hospital network restrictions |
| Full product page redesign | HTML restructure too large for polish milestone |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| TYPO-01 | Phase 4 | Complete |
| TYPO-02 | Phase 4 | Complete |
| TYPO-03 | Phase 4 | Complete |
| TYPO-04 | Phase 4 | Complete |
| HOVR-01 | Phase 4 | Complete |
| ANIM-01 | Phase 5 | Pending |
| ANIM-02 | Phase 5 | Pending |
| ANIM-03 | Phase 5 | Pending |
| HERO-03 | Phase 5 | Pending |
| HERO-01 | Phase 6 | Pending |
| HERO-02 | Phase 6 | Pending |

**Coverage:**
- v1.1 requirements: 11 total
- Mapped to phases: 11
- Unmapped: 0

---
*Requirements defined: 2026-02-18*
*Last updated: 2026-02-18 after Phase 4 completion*
