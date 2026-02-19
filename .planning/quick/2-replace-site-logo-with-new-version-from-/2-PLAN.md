---
phase: quick-2
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - img/logo.png
  - index.html
  - empresa.html
  - produtos.html
  - localizacao.html
  - contato.html
  - css/style.css
autonomous: true
requirements: []
must_haves:
  truths:
    - "All 5 pages display the new logo image instead of plain text"
    - "Logo links to index.html on all pages"
    - "Logo scales appropriately on mobile and desktop"
  artifacts:
    - path: "img/logo.png"
      provides: "Logo image file"
    - path: "css/style.css"
      provides: "Updated logo styles for image display"
      contains: ".logo img"
  key_links:
    - from: "*.html"
      to: "img/logo.png"
      via: "img tag src attribute"
      pattern: 'src="img/logo.png"'
---

<objective>
Replace the text-only "MetaMedical" logo with the actual logo image from https://metamedical.com.br/img/green/logo.png across all 5 site pages.

Purpose: Upgrade from placeholder text logo to the real brand logo image.
Output: Logo image downloaded and integrated into all pages with appropriate styling.
</objective>

<execution_context>
@/home/thiago/.claude/get-shit-done/workflows/execute-plan.md
@/home/thiago/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@css/style.css
@index.html
</context>

<tasks>

<task type="auto">
  <name>Task 1: Download logo and update all HTML pages and CSS</name>
  <files>img/logo.png, index.html, empresa.html, produtos.html, localizacao.html, contato.html, css/style.css</files>
  <action>
1. Create `img/` directory and download the logo:
   ```
   mkdir -p img
   curl -o img/logo.png https://metamedical.com.br/img/green/logo.png
   ```

2. In ALL 5 HTML files (index.html, empresa.html, produtos.html, localizacao.html, contato.html), replace line 22:
   ```html
   <a href="index.html" class="logo">MetaMedical</a>
   ```
   with:
   ```html
   <a href="index.html" class="logo"><img src="img/logo.png" alt="MetaMedical" height="40"></a>
   ```

3. In `css/style.css`, update the `.logo` block (lines 177-184) to work with an image instead of styled text. Replace:
   ```css
   .logo {
     font-family: var(--font-heading);
     font-size: 1.5rem;
     font-weight: 700;
     color: var(--color-primary);
     letter-spacing: -0.02em;
     white-space: nowrap;
   }

   .logo:hover {
     color: var(--color-primary-dark);
   }
   ```
   with:
   ```css
   .logo {
     display: flex;
     align-items: center;
     white-space: nowrap;
   }

   .logo img {
     height: 40px;
     width: auto;
     display: block;
   }

   .logo:hover {
     opacity: 0.8;
   }
   ```
  </action>
  <verify>
  - `ls -la img/logo.png` confirms file exists and has non-zero size
  - `grep -c 'img/logo.png' *.html` returns 1 for each of the 5 HTML files
  - `grep -c '.logo img' css/style.css` returns at least 1
  - Open index.html in browser to visually confirm logo displays
  </verify>
  <done>All 5 pages show the downloaded logo image instead of text, logo links to index.html, and hover state works with opacity transition.</done>
</task>

</tasks>

<verification>
- Logo image file exists at img/logo.png
- All 5 HTML files reference img/logo.png in the header
- CSS styles the logo as an image (not text)
- No remaining text-only "MetaMedical" in the logo anchor tags
</verification>

<success_criteria>
- img/logo.png downloaded and present in repo
- All 5 HTML pages display the image logo in the header
- Logo is a clickable link to index.html on every page
- Logo renders cleanly at appropriate size on desktop and mobile
</success_criteria>

<output>
After completion, create `.planning/quick/2-replace-site-logo-with-new-version-from-/2-SUMMARY.md`
</output>
