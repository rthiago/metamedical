# Phase 8: GitHub Pages Deployment - Context

**Gathered:** 2026-02-19
**Status:** Ready for planning

<domain>
## Phase Boundary

Configure GitHub Pages deployment via GitHub Actions so the site is publicly accessible. The repository already exists on GitHub with the full codebase (Phase 7). This phase adds the deployment workflow and verifies the site is live.

</domain>

<decisions>
## Implementation Decisions

### Branch name in workflow
- The GitHub Actions workflow MUST trigger on push to `master` (not `main`)
- User decided in Phase 7 to keep `master` as the branch name
- The ROADMAP success criteria mentions "push to main" but this should be read as "push to the default branch" which is `master`

### Repository visibility
- The repository is currently private (Phase 7 decision)
- GitHub Pages for private repos requires a paid GitHub plan
- If Pages deployment fails due to private repo, switch to public — the site content is meant to be publicly accessible anyway
- This is an informational website with no secrets in the codebase

### Deployment method
- Use GitHub Actions with the official `actions/deploy-pages` workflow
- Use the `actions/upload-pages-artifact` + `actions/deploy-pages` pattern (standard for static sites)
- Workflow file at `.github/workflows/deploy.yml`
- Deploy the entire repository root (no build step needed — plain HTML/CSS/JS)

### Site URL
- Default GitHub Pages URL: `https://rthiago.github.io/metamedical/`
- Asset paths in HTML must work with the `/metamedical/` base path
- Check for absolute vs relative paths in CSS/JS/HTML references

### Claude's Discretion
- Exact workflow YAML structure and job naming
- Whether to add a workflow status badge
- How to handle path prefixing if needed (base href or relative paths)

</decisions>

<specifics>
## Specific Ideas

- Site must render correctly at the GitHub Pages URL (all pages, images, styles, scripts)
- Verify navigation links work with the repository subdirectory path
- The Google Maps embed and external CDN resources (Google Fonts, product images) should continue working

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 08-github-pages-deployment*
*Context gathered: 2026-02-19*
