# CLAUDE.md

Official Jamdesk documentation — served at `jamdesk.com/docs`.

> **Build internals:** For MDX components, themes, and build service details, see `builder/CLAUDE.md`.

## Development

**NEVER start a dev server without explicit user permission.**

```bash
# From the jamdesk monorepo root:
cd builder/build-service && node scripts/dev-project.cjs jamdesk-docs
```

Dev server runs at `http://localhost:3000/introduction`. This project does NOT have `hostAtDocs` in docs.json — it's set at infrastructure level (Cloudflare Worker + middleware), so local dev serves at root, not `/docs`.

**Sidebar titles from frontmatter** only update on dev server restart (enhance-navigation runs once at startup).

## Structure

Two tabs: **Docs** (technical documentation) and **Help Center** (dashboard/product support).

```
jamdesk-docs/
├── docs.json              # Navigation, theme (jam), colors, redirects
├── introduction.mdx       # Root pages
├── quickstart.mdx
├── setup/                 # Project setup, analytics, GitHub, monorepo, redirects
├── ai/                    # AI features (chat, MCP, llms.txt, Claude Code, Cursor, Codex)
├── components/            # 22 MDX component pages (card, tabs, accordion, steps, etc.)
├── content/               # Writing guides (code blocks, frontmatter, MDX, SEO, snippets)
├── customization/         # Theming, branding, custom CSS
├── deploy/                # Deployment (custom domains, Vercel, Cloudflare, AWS, reverse proxy)
├── development/           # Local preview, VS Code extension
├── cli/                   # CLI docs (overview, auth, deploy)
├── api-reference/         # OpenAPI example pages
├── navigation/            # Navigation configuration
├── reference/             # Changelog
├── help/                  # Help Center (dashboard guides, troubleshooting, billing, FAQ)
│   ├── getting-started/   # Onboarding, first build, dashboard tour
│   ├── account/           # Settings, password, GitHub linking
│   ├── projects/          # Creating, settings, team members, ownership transfer
│   ├── builds/            # Triggering, monitoring, troubleshooting
│   ├── billing/           # Plans, subscription management
│   ├── troubleshooting/   # Error reference, DNS, build failures, login issues
│   └── support/           # Contact, security
├── openapi/               # OpenAPI spec files
└── images/                # Logos, favicons
```

## Writing Guidelines

- Every page needs `title` and `description` frontmatter
- Start with the why, then show how — working examples for every feature
- Use `<Columns>`, `<Card>`, `<Tabs>`, `<Steps>`, `<Accordion>`, `<Note>`/`<Warning>`/`<Tip>` — all globally available
- Page paths in `docs.json` navigation omit `.mdx` extension
- Icons use Font Awesome names (e.g., `book-open`, `code`, `rocket`)
- Link to related pages at bottom with "What's Next?" cards

## Adding Pages

1. Create `.mdx` file in the appropriate directory
2. Add page path to `docs.json` navigation (under the correct tab/group)
3. Add "What's Next?" cards from related pages

## Deployment

Auto-deploys when pushed to connected GitHub branch. Manual rebuild via dashboard.

---
*Last reviewed: 2026-02-28*
