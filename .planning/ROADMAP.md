# Roadmap: MetaMedical Website Redesign

## Overview

Three phases to deliver a modern institutional website for MetaMedical. Phase 1 establishes the shared site shell and design system. Phase 2 fills every page with its content. Phase 3 adds the finishing layer of SEO metadata, CSS animations, and confirms deploy-readiness.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Foundation** - Site shell, design system, shared header/nav/footer
- [x] **Phase 2: Content Pages** - All five pages filled with MetaMedical content
- [ ] **Phase 3: Polish & Deploy** - SEO tags, animations, final deploy readiness

## Phase Details

### Phase 1: Foundation
**Goal**: A deployable site shell exists with consistent navigation, footer, and design language
**Depends on**: Nothing (first phase)
**Requirements**: STRC-01, STRC-02, STRC-03, STRC-04, STRC-05, DSGN-01, DSGN-02, DSGN-03, DSGN-05
**Success Criteria** (what must be TRUE):
  1. Opening any of the 5 pages on a browser shows a consistent header with logo and nav links
  2. On mobile, the navigation collapses to a hamburger menu that opens and closes
  3. Every page shows the same footer with CNPJ, address, and RE 8365-SP
  4. The site uses a professional medical color scheme with Google Fonts typography
  5. The codebase is plain HTML and CSS files with no build step required
**Plans**: 1 plan

Plans:
- [x] 01-01-PLAN.md — Build site shell with CSS design system, shared header/nav/footer, and 5 HTML page stubs

### Phase 2: Content Pages
**Goal**: Every page displays complete MetaMedical content so visitors can learn about the company, products, location, and contact
**Depends on**: Phase 1
**Requirements**: HOME-01, HOME-02, HOME-03, EMPR-01, EMPR-02, PROD-01, PROD-02, PROD-03, PROD-04, PROD-05, LOCL-01, LOCL-02, CONT-01, CONT-02
**Success Criteria** (what must be TRUE):
  1. Home page shows hero with "Compromisso com a saúde" tagline, 4 value pillars, and a product preview linking to Produtos
  2. Empresa page displays institutional content and company values with descriptions
  3. Produtos page shows cards for Bleed STP, Pinça Bipolar, and Bleed STP+ each with image and description, plus a catalog request prompt
  4. Localização page shows the full address and an embedded map of Osasco/SP
  5. Contato page clearly displays phone, email, and address with no form
**Plans**: 2 plans

Plans:
- [x] 02-01-PLAN.md — Build Home and Empresa pages with hero, values, product preview, and institutional content
- [x] 02-02-PLAN.md — Build Produtos, Localização, and Contato pages with product cards, map, and contact info

### Phase 3: Polish & Deploy
**Goal**: The site is search-engine ready with smooth interactions and can be uploaded to shared hosting as-is
**Depends on**: Phase 2
**Requirements**: DSGN-04, SEO-01, SEO-02, SEO-03
**Success Criteria** (what must be TRUE):
  1. Every page has a unique title, meta description, and Open Graph tags visible in page source
  2. HTML structure uses semantic elements (header, nav, main, section, footer) throughout
  3. Interactive elements (nav links, product cards, buttons) have visible hover/transition effects
  4. Uploading the files folder to a web host produces a working site with no configuration needed
**Plans**: TBD

Plans:
- [ ] 03-01: Add SEO meta/OG tags, CSS animations, and verify deploy readiness

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation | 1/1 | Complete | 2026-02-18 |
| 2. Content Pages | 2/2 | Complete | 2026-02-18 |
| 3. Polish & Deploy | 0/1 | Not started | - |
