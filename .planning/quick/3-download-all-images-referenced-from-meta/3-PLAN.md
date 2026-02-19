---
phase: quick-3
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - img/medical-slide-1.jpg
  - img/prod-1.jpg
  - img/prod-1-2.jpg
  - img/prod-2.jpg
  - img/prod-2-2.jpg
  - img/prod-3.jpg
  - index.html
  - produtos.html
  - css/style.css
autonomous: true
requirements: [QUICK-3]

must_haves:
  truths:
    - "All 6 product and hero images load from local img/ directory"
    - "No remaining references to metamedical.com.br/pic/ in any HTML or CSS file"
    - "Site renders identically with locally-served images"
  artifacts:
    - path: "img/medical-slide-1.jpg"
      provides: "Hero/overlay background image"
    - path: "img/prod-1.jpg"
      provides: "Bleed STP product image (homepage)"
    - path: "img/prod-1-2.jpg"
      provides: "Bleed STP product image (product page)"
    - path: "img/prod-2.jpg"
      provides: "Pinca Bipolar product image (homepage)"
    - path: "img/prod-2-2.jpg"
      provides: "Pinca Bipolar product image (product page)"
    - path: "img/prod-3.jpg"
      provides: "Bleed STP+ product image (both pages)"
  key_links:
    - from: "index.html"
      to: "img/prod-*.jpg"
      via: "relative src attributes"
      pattern: 'src="img/prod-'
    - from: "produtos.html"
      to: "img/prod-*.jpg"
      via: "relative src attributes and background-image url"
      pattern: 'src="img/prod-'
    - from: "css/style.css"
      to: "img/medical-slide-1.jpg"
      via: "background url()"
      pattern: "url\\('../img/medical-slide-1.jpg'\\)"
---

<objective>
Download all 6 images currently referenced from metamedical.com.br/pic/ and update all HTML/CSS references to use local relative paths from the img/ directory.

Purpose: Eliminate external image dependency so the site is fully self-contained and loads faster (no cross-origin requests to metamedical.com.br).
Output: 6 image files in img/, updated references in index.html, produtos.html, css/style.css.
</objective>

<execution_context>
@/home/thiago/.claude/get-shit-done/workflows/execute-plan.md
@/home/thiago/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/STATE.md
</context>

<tasks>

<task type="auto">
  <name>Task 1: Download all 6 images from metamedical.com.br/pic/</name>
  <files>img/medical-slide-1.jpg, img/prod-1.jpg, img/prod-1-2.jpg, img/prod-2.jpg, img/prod-2-2.jpg, img/prod-3.jpg</files>
  <action>
Download all 6 images from metamedical.com.br into the img/ directory using curl or wget:

```
https://metamedical.com.br/pic/medical-slide-1.jpg -> img/medical-slide-1.jpg
https://metamedical.com.br/pic/prod-1.jpg         -> img/prod-1.jpg
https://metamedical.com.br/pic/prod-1-2.jpg       -> img/prod-1-2.jpg
https://metamedical.com.br/pic/prod-2.jpg         -> img/prod-2.jpg
https://metamedical.com.br/pic/prod-2-2.jpg       -> img/prod-2-2.jpg
https://metamedical.com.br/pic/prod-3.jpg         -> img/prod-3.jpg
```

Verify each downloaded file is a valid JPEG (non-zero size, correct file type header).
  </action>
  <verify>All 6 files exist in img/ with non-zero size: `ls -la img/*.jpg` shows 6 JPG files (plus logo.png already there). Run `file img/*.jpg` to confirm they are valid JPEG images.</verify>
  <done>6 JPEG image files downloaded and present in img/ directory.</done>
</task>

<task type="auto">
  <name>Task 2: Update all HTML and CSS references to use local paths</name>
  <files>index.html, produtos.html, css/style.css</files>
  <action>
Replace all external image URLs with local relative paths:

In index.html (3 replacements):
- `https://metamedical.com.br/pic/prod-1.jpg` -> `img/prod-1.jpg`
- `https://metamedical.com.br/pic/prod-2.jpg` -> `img/prod-2.jpg`
- `https://metamedical.com.br/pic/prod-3.jpg` -> `img/prod-3.jpg`

In produtos.html (4 replacements):
- `https://metamedical.com.br/pic/prod-1-2.jpg` -> `img/prod-1-2.jpg`
- `https://metamedical.com.br/pic/prod-2-2.jpg` -> `img/prod-2-2.jpg`
- `https://metamedical.com.br/pic/prod-3.jpg` -> `img/prod-3.jpg`
- `https://metamedical.com.br/pic/medical-slide-1.jpg` -> `img/medical-slide-1.jpg`

In css/style.css (1 replacement):
- `url('https://metamedical.com.br/pic/medical-slide-1.jpg')` -> `url('../img/medical-slide-1.jpg')`

Note: CSS url() path needs `../img/` because style.css is in css/ subdirectory.

Do NOT change og:url meta tags or email addresses -- only image src/url references to /pic/ paths.
  </action>
  <verify>Run `grep -r 'metamedical.com.br/pic/' *.html css/*.css` -- must return zero results. Run `grep -c 'img/prod-\|img/medical-' index.html produtos.html css/style.css` to confirm local references exist.</verify>
  <done>All 8 image references across 3 files point to local img/ paths. Zero remaining references to metamedical.com.br/pic/.</done>
</task>

</tasks>

<verification>
1. `grep -r 'metamedical.com.br/pic/' *.html css/*.css` returns no results
2. `ls img/*.jpg | wc -l` returns 6
3. `file img/*.jpg` confirms all are valid JPEG images
4. Open index.html in browser -- all 3 product images render
5. Open produtos.html in browser -- all 3 product images and background overlay render
</verification>

<success_criteria>
- All 6 images served locally from img/ directory
- No external metamedical.com.br/pic/ references remain in HTML or CSS
- Site renders identically with local images
</success_criteria>

<output>
After completion, create `.planning/quick/3-download-all-images-referenced-from-meta/3-SUMMARY.md`
</output>
