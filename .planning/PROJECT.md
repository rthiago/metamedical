# MetaMedical Website Redesign

## What This Is

A modern institutional website for MetaMedical.com.br — company info, products (Bleed STP, Pinça Bipolar, Bleed STP+), location, and contact details — with a professional medical/corporate design, premium typography, scroll animations, and image overlays. Built with plain HTML, CSS, and vanilla JavaScript, deployable to any shared web host.

## Core Value

Visitors immediately understand what MetaMedical does and can find product information and contact details with zero friction.

## Requirements

### Validated

- ✓ Modern corporate medical design (clean, trustworthy, professional) — v1.0
- ✓ Same page structure: Home, Empresa, Produtos, Localização, Contato — v1.0
- ✓ Same product information: Bleed STP, Pinça Bipolar, Bleed STP+ — v1.0
- ✓ Company values and institutional content preserved — v1.0
- ✓ Contact page with phone, email, address (no form) — v1.0
- ✓ Mobile-responsive layout — v1.0
- ✓ Portuguese-only content — v1.0
- ✓ Plain HTML/CSS — no frameworks, no build tools — v1.0
- ✓ Deployable to any standard web host — v1.0
- ✓ Fluid clamp() headings scaling smoothly mobile to desktop — v1.1
- ✓ Generous spacing (6rem+) for premium breathing room — v1.1
- ✓ Cards with 8px radius and layered box-shadows — v1.1
- ✓ H2 accent bars for visual rhythm — v1.1
- ✓ Unified transition timing across all interactive elements — v1.1
- ✓ Hero brand-tinted gradient overlay — v1.1
- ✓ Reusable overlay-section CSS pattern — v1.1
- ✓ Hero staggered fade-up entrance — v1.1
- ✓ Scroll reveal via IntersectionObserver — v1.1
- ✓ prefers-reduced-motion accessibility support — v1.1
- ✓ No-JS fallback (all content visible without JavaScript) — v1.1

### Active

- [x] Private GitHub repository with full project history — v1.2 Phase 7
- [x] Site deployed and accessible via GitHub Pages — v1.2 Phase 8
- [x] GitHub Actions workflow for automated deployment — v1.2 Phase 8

### Out of Scope

- E-commerce / shopping cart — informational site only
- Contact form — info display only, no backend needed
- English translation — Portuguese only
- Customer login / catalog request system — not needed
- CMS / admin panel — static files only
- WhatsApp integration — keeping it simple
- Parallax scrolling — motion sickness risk, dated feel, poor mobile performance
- CSS scroll-driven animations — Firefox lacks support
- Animation libraries (AOS, GSAP) — vanilla JS IntersectionObserver suffices
- Dark mode — not expected for medical B2B audience

## Context

- Shipped v1.1 with 1,496 LOC (HTML + CSS + JS), 5 pages, fully responsive
- Source code hosted at github.com/rthiago/metamedical (public, 75+ commits)
- Site live at https://rthiago.github.io/metamedical/ (GitHub Pages, auto-deployed)
- Tech stack: Plain HTML, CSS, vanilla JavaScript, Google Fonts (CDN), Google Maps embed
- Company is based in Osasco, SP, Brazil (CNPJ 21.033.671/0001-29)
- Products are medical devices/supplies — professional, clinical positioning
- Target audience: healthcare professionals and institutions looking for medical supplies
- Site replaces previous Cedesign-built site at metamedical.com.br
- Visual polish complete: fluid typography, scroll reveals, image overlays, premium card styling

## Constraints

- **Tech stack**: Plain HTML, CSS, and vanilla JavaScript — no frameworks, no build step
- **Content**: Preserve existing information — this is a design refresh, not a content rewrite
- **Language**: Brazilian Portuguese only
- **Hosting**: Must work on simple shared hosting (static file upload)

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Plain HTML/CSS over frameworks | Simplicity, easy hosting, no build tools needed | ✓ Good — fast development, zero config deploy |
| Info-only contact page | No backend complexity, matches current needs | ✓ Good — clean layout |
| Portuguese only | Current audience is Brazilian, no international need | ✓ Good |
| CSS-only hamburger menu (checkbox hack) | No JavaScript needed for mobile nav | ✓ Good — works without JS |
| Mobile-first responsive (768px breakpoint) | Most users browse on mobile | ✓ Good |
| Google Fonts via CDN | Simple, no self-hosting needed | ✓ Good |
| Product images from metamedical.com.br CDN | Reuse existing assets | ✓ Good — no image hosting needed |
| Google Maps embed via query URL | No API key required | ✓ Good — zero-config |
| Vanilla JS allowed for v1.1 | Enables scroll reveals and smooth animations | ✓ Good — 28 LOC, progressive enhancement |
| CSS clamp() for fluid typography | Browser-native, zero-dependency responsive sizing | ✓ Good — smooth scaling |
| Layered box-shadows for card depth | Multi-layer shadows create realistic elevation | ✓ Good — premium feel |
| H2 accent bars via ::before | No HTML changes needed, pure CSS presentation | ✓ Good — visual rhythm |
| cubic-bezier(0.4,0,0.2,1) for transitions | Material standard curve, snappier than ease | ✓ Good — responsive feel |
| Progressive enhancement via .js-enabled class | Content visible without JS, animations layered on top | ✓ Good — accessible |
| Reveal-once scroll pattern | Elements unobserved after reveal, no re-trigger | ✓ Good — clean UX |
| Hero CSS @keyframes over IntersectionObserver | Hero always visible, doesn't need scroll trigger | ✓ Good — immediate impact |
| Overlay-section with CSS custom properties | Configurable gradient per section instance | ✓ Good — reusable pattern |
| Private GitHub repository | No public exposure needed; can switch to public later | ✓ Good — privacy maintained |
| Keep master branch name | Existing history uses master, no reason to rename | ✓ Good — no disruption |
| Include .planning/ in repo | Planning docs are valuable project history | ✓ Good — full context preserved |
| Switch repo to public for Pages | GitHub Pages requires public on free plan | ✓ Good — content is public-facing anyway |
| GitHub Actions deploy workflow | Official actions/deploy-pages pattern | ✓ Good — automated, zero-config |

## Completed Milestones

- **v1.0 MVP** (Phases 1-3) — shipped 2026-02-19
- **v1.1 Visual Polish** (Phases 4-6) — shipped 2026-02-19
- **v1.2 GitHub Pages Deployment** (Phases 7-8) — shipped 2026-02-19

**Live site:** https://rthiago.github.io/metamedical/

---
*Last updated: 2026-02-19 after v1.2 milestone complete*
