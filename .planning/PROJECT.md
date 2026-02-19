# MetaMedical Website Redesign

## What This Is

A modern institutional website for MetaMedical.com.br — company info, products (Bleed STP, Pinça Bipolar, Bleed STP+), location, and contact details — with a professional medical/corporate design. Built with plain HTML and CSS, deployable to any shared web host.

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

### Active

#### Current Milestone: v1.1 Visual Polish

**Goal:** Comprehensive visual refresh — premium feel with image overlays, product grids, bold typography, scroll animations, and refined spacing.

**Target features:**
- ~~Bolder display typography and stronger hierarchy~~ ✓ Phase 4
- ~~Generous whitespace and clearer section rhythm~~ ✓ Phase 4
- ~~Smoother hover effects and transitions~~ ✓ Phase 4
- Hero sections with image overlays (text + CTA over large photos)
- Scroll reveal animations (vanilla JS)
- Product grid redesign (clean image+text pairs)

### Out of Scope

- E-commerce / shopping cart — informational site only
- Contact form — info display only, no backend needed
- English translation — Portuguese only
- Customer login / catalog request system — not needed
- CMS / admin panel — static files only
- WhatsApp integration — keeping it simple

## Context

- Shipped v1.0 with 1,324 LOC (HTML + CSS), 5 pages, fully responsive
- Tech stack: Plain HTML, CSS, Google Fonts (CDN), Google Maps embed
- Company is based in Osasco, SP, Brazil (CNPJ 21.033.671/0001-29)
- Products are medical devices/supplies — professional, clinical positioning
- Target audience: healthcare professionals and institutions looking for medical supplies
- Site replaces previous Cedesign-built site at metamedical.com.br

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
| Vanilla JS allowed for v1.1 | Enables scroll reveals and smooth animations | — Pending |
| CSS clamp() for fluid typography | Browser-native, zero-dependency responsive sizing | ✓ Good — smooth scaling |
| Layered box-shadows for card depth | Multi-layer shadows create realistic elevation | ✓ Good — premium feel |
| H2 accent bars via ::before | No HTML changes needed, pure CSS presentation | ✓ Good — visual rhythm |
| cubic-bezier(0.4,0,0.2,1) for transitions | Material standard curve, snappier than ease | ✓ Good — responsive feel |

---
*Last updated: 2026-02-18 after Phase 4 completion*
