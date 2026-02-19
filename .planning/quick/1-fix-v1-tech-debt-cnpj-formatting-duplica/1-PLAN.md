---
phase: quick
plan: 1
type: execute
wave: 1
depends_on: []
files_modified:
  - index.html
  - empresa.html
  - produtos.html
  - localizacao.html
  - contato.html
  - css/style.css
autonomous: true
requirements: [TECH-DEBT-V1]
must_haves:
  truths:
    - "All 5 page footers display CNPJ as 21.033.671/0001-29 with hyphen"
    - "Google Fonts CSS is loaded exactly once per page (link tag only, no @import)"
    - "4th value pillar on index.html reads 'Tire suas d&uacute;vidas' not 'Suporte'"
  artifacts:
    - path: "index.html"
      provides: "Fixed CNPJ in footer, corrected value pillar label"
      contains: "0001-29"
    - path: "css/style.css"
      provides: "No duplicate @import for Google Fonts"
  key_links: []
---

<objective>
Fix three v1 tech debt items: CNPJ formatting missing hyphen in all 5 footers, duplicate Google Fonts @import in style.css, and incorrect value pillar label on home page.

Purpose: Clean up known defects from v1 milestone audit before shipping.
Output: All 6 files corrected, zero tech debt remaining.
</objective>

<execution_context>
@/home/thiago/.claude/get-shit-done/workflows/execute-plan.md
@/home/thiago/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/STATE.md
@.planning/v1-MILESTONE-AUDIT.md
</context>

<tasks>

<task type="auto">
  <name>Task 1: Fix CNPJ formatting, remove duplicate font import, fix pillar label</name>
  <files>index.html, empresa.html, produtos.html, localizacao.html, contato.html, css/style.css</files>
  <action>
1. In ALL 5 HTML files (index.html, empresa.html, produtos.html, localizacao.html, contato.html), find the footer CNPJ line containing `21.033.671/000129` and replace with `21.033.671/0001-29` (add hyphen before check digits).

2. In css/style.css, DELETE line 7 — the `@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Montserrat:wght@600;700&display=swap');` line. All HTML files already load this font via `<link>` tags in `<head>`, so the @import is a duplicate causing an extra network request.

3. In index.html, find the 4th value pillar card (the one with `&#x1F4AC;` icon and `<h3>Suporte</h3>`) and change the heading to `<h3>Tire suas d&uacute;vidas</h3>` to match the HOME-02 requirement.
  </action>
  <verify>
- grep -c "000129" *.html should return 0 for all files (no old format remaining)
- grep -c "0001-29" *.html should return 1 for each of the 5 files (footer) plus 1 extra in contato.html (body), totaling 6 matches
- grep -c "@import" css/style.css should return 0
- grep "Tire suas" index.html should show the corrected pillar label
- grep "Suporte" index.html should return nothing
  </verify>
  <done>All 5 footers show correct CNPJ format 21.033.671/0001-29, Google Fonts loaded once per page via link tag only, 4th value pillar reads "Tire suas duvidas"</done>
</task>

</tasks>

<verification>
- All HTML files pass: no instance of `000129` (without hyphen) in any footer
- css/style.css has zero `@import` lines
- index.html 4th value card heading is "Tire suas duvidas", not "Suporte"
- Pages render correctly in browser (no broken fonts, no layout changes)
</verification>

<success_criteria>
- CNPJ displays as `21.033.671/0001-29` in all 5 page footers
- Single Google Fonts load per page (link tag in head, no CSS @import)
- Home page 4th value pillar labeled "Tire suas duvidas"
</success_criteria>

<output>
After completion, create `.planning/quick/1-fix-v1-tech-debt-cnpj-formatting-duplica/1-SUMMARY.md`
</output>
