# Jamdesk Docs Audit - Improvement Backlog (2026-02-03)

## Acceptance Criteria

| Issue Type | Definition of Done |
|------------|-------------------|
| Thin intro | 2-3 sentences (50-120 words) stating problem, audience, and outcome |
| No internal links | At least 2 contextual links to related pages |
| No next-steps section | "What's Next" or "Related" block with 2-4 cards/links |
| Missing code example | Minimal runnable snippet demonstrating primary use case |
| H1 in body | No `#` headings in body; all sections use `##` or lower |
| Duplicate title | Disambiguate with suffix: `(Docs)` / `(Help)` or fuller descriptive title |

## P1 (High Impact)

_Order matters: complete in sequence due to dependencies._

- [x] **1.1** Create a true Deployment overview page that covers default subdomain hosting, custom domains, and subpath hosting; move current `/deploy/overview` content to a dedicated "Subpath Hosting" page and update navigation to match reader expectations.
- [x] **1.2** Add a dedicated "Builds API / Build Triggers" reference page that consolidates rebuild endpoints, auth, rate limits, and examples now scattered across Help and Development pages.
- [x] **1.3** Unify Analytics guidance by adding a developer-facing "Analytics Overview" page and cross-linking to Help Center troubleshooting; reduce duplication between `setup/*-analytics` and `help/analytics/*`.
- [ ] **1.4** Create an end-to-end tutorial guide: “Create → Preview → Deploy → Custom Domain,” and link it from Quickstart and Setup.

## P2 (Medium Impact)

- [x] **2.1** Add a consistent "What's Next" block to pages missing next steps (55 pages). Start with Components, Deploy, and Content sections, which have the lowest next-step coverage. _Depends on: P1.1 (deployment links will change)._
- [x] **2.2** Add internal links to pages with none (23 pages), especially deploy provider pages and component docs. Cross-link back to overviews and troubleshooting.
- [x] **2.3** Disambiguate duplicate titles across Docs and Help Center. Use pattern: `Title (Docs)` / `Title (Help)` or expand to fuller titles (e.g., "Creating Projects in Dashboard" vs "Creating Projects via CLI"). Affected: `setup/creating-projects.mdx`, `setup/search-analytics.mdx`, `help/projects/creating.mdx`, `help/analytics/search-analytics.mdx`.
- [x] **2.4** Add minimum actionable examples to docs pages missing code or concrete walkthroughs: `setup/creating-projects.mdx`, `setup/migration.mdx`, `setup/project-analytics.mdx`, `setup/search-analytics.mdx`, `development/ai-writing.mdx`, `components/overview.mdx`.
- [x] **2.5** Standardize component pages with a consistent "Props" section pattern and a short "Usage" snippet to improve predictability.
- [x] **2.6** Run broken link audit to verify all existing internal links resolve (not just "no links" pages).
- [ ] **2.7** Add missing code examples to remaining pages: `setup/analytics-overview.mdx`, `deploy/overview.mdx`, `help/github-integration.mdx`.
- [ ] **2.8** Add step-by-step guidance to pages missing procedures: `help/getting-started/onboarding.mdx`, `help/billing/plans.mdx`.

## P3 (Low Impact)

- [x] **3.1** Remove H1 headings in body content (6 pages total—can be a single regex pass):
  - `quickstart.mdx`
  - `development/ai-documentation.mdx`
  - `development/mcp-integration.mdx`
  - `deploy/cloudflare.mdx`
  - `content/mdx-basics.mdx`
  - `help/custom-domains.mdx`
- [ ] **3.2** Normalize CTA phrasing across Docs and Help Center for consistency ("Get Started," "Quickstart," "Create Project").
- [ ] **3.3** Expand intros on thin pages (<120 characters) with a 2–3 sentence context + audience framing.
- [x] **3.4** Audit images/media: check for missing alt text, broken images, and outdated screenshots.

## SEO Readiness (Indexing Off)

_Prerequisite for enabling search indexing. Complete P2.1-P2.3 first to establish link structure._

- [ ] **SEO.1** Align title and description lengths across all pages (target ~50–60 chars for titles, ~120–160 for descriptions) before enabling indexing.
- [x] **SEO.2** Add internal links and next steps to strengthen crawl paths and topical clustering. _Covered by P2.1 and P2.2._
- [ ] **SEO.3** Confirm canonical strategy and sitemap coverage once indexing is enabled.

## Validation

- [x] After changes, re-run the audit scripts to confirm reduced counts for missing next steps, internal links, and missing code examples.
- [x] Spot-check navigation flow for the Deploy and Analytics sections to ensure clarity.
- [x] Verify no broken internal links (run link checker).
