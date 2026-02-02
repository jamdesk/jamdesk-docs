# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **official Jamdesk documentation** - a content project containing MDX files that document the Jamdesk platform itself. It's built by the Jamdesk builder service and served at `jamdesk.com/docs`.

> **Build internals:** For MDX components, themes, and build service details, see `builder/CLAUDE.md`.

## Development Commands

```bash
# From the jamdesk monorepo root (../..):
cd builder/build-service && node scripts/dev-project.cjs jamdesk-docs

# Or using the builder CLI (if linked):
cd builder && ./jd dev jamdesk-docs
```

The dev server runs at `http://localhost:3000/introduction` by default.

### IMPORTANT: Development Server

**NEVER start a development server without explicit user permission.** Always ask the user first before running any dev server command, even in auto or bypass mode. The user may already have a server running in another terminal.

If you need to test changes, ask: "Should I start the dev server, or do you already have one running?"

## Project Structure

```
jamdesk-docs/
├── docs.json           # Site configuration (theme, colors, navigation)
├── introduction.mdx    # Root-level pages
├── quickstart.mdx
├── how-jamdesk-works.mdx
├── cli/                # CLI documentation
│   └── overview.mdx
├── components/         # Component documentation
│   ├── overview.mdx
│   ├── card.mdx
│   ├── tabs.mdx
│   ├── accordion.mdx
│   ├── steps.mdx
│   ├── expandable.mdx
│   ├── frame.mdx
│   └── code-group.mdx
├── config/             # Configuration reference
│   └── docs-json-reference.mdx
├── content/            # Writing content guides
│   └── code-blocks.mdx
├── navigation/         # Navigation docs
│   └── overview.mdx
└── images/             # Logo and favicon SVGs
```

## Key Files

- **docs.json**: Central configuration - theme (`jam`), colors, navigation structure, branding
- **MDX files**: Documentation pages with frontmatter (`title`, `description`)

## Writing Great Documentation

### Page Structure

Every page should follow this pattern:

```mdx
---
title: Page Title
description: Brief description for SEO and previews
---

Opening paragraph explaining what this page covers.

## First Section

Content organized logically with clear headings.

## What's Next?

<Columns cols={2}>
  <Card title="Related Topic" icon="icon-name" href="/path">
    Brief description
  </Card>
</Columns>
```

### Writing Style

1. **Start with the why** - Explain what problem something solves before how to use it
2. **Use progressive disclosure** - Simple examples first, advanced options later
3. **Show, don't tell** - Include working examples for every feature
4. **Be concise** - One idea per paragraph, remove unnecessary words
5. **Use active voice** - "Run this command" not "This command should be run"

### Code Examples

Always include complete, runnable examples:

```mdx
```bash
# Good: Complete command with context
jamdesk dev --port 3001
```

```bash
# Bad: Incomplete or unclear
dev -p
```
```

### Component Usage Best Practices

**Tabs** - Use for mutually exclusive options (npm vs yarn, languages):

```mdx
<Tabs>
  <Tab title="npm">npm install jamdesk</Tab>
  <Tab title="yarn">yarn add jamdesk</Tab>
</Tabs>
```

**Accordions** - Use for optional/advanced content that shouldn't clutter the page:

```mdx
<Accordion title="Advanced Configuration" icon="gear">
  Optional details that power users might need.
</Accordion>
```

**Steps** - Use for sequential procedures:

```mdx
<Steps>
  <Step title="Install">First step</Step>
  <Step title="Configure">Second step</Step>
  <Step title="Run">Final step</Step>
</Steps>
```

**Cards** - Use for navigation and feature highlights:

```mdx
<Columns cols={2}>
  <Card title="Quick Start" icon="rocket" href="/quickstart">
    Get up and running in 5 minutes
  </Card>
</Columns>
```

**Callouts** - Use sparingly for important information:

```mdx
<Note>Helpful context or tips</Note>
<Warning>Important caveats or requirements</Warning>
<Tip>Optional optimization or best practice</Tip>
```

## Available MDX Components

Components are globally available in all MDX files:

**Layout**: `Card`, `Columns`, `Tabs`, `Tab`, `Accordion`, `AccordionGroup`, `Steps`, `Step`, `Expandable`, `Frame`, `CodeGroup`

**Callouts**: `Note`, `Info`, `Warning`, `Tip`, `Check`, `Danger`

**API**: `RequestExample`, `ResponseExample`

## Content Conventions

- Page paths in `docs.json` navigation omit the `.mdx` extension
- Use relative paths from root for page references (e.g., `components/card`)
- Icons use Font Awesome Light variants (e.g., `book-open`, `code`, `rocket`)
- Images go in `/images/` and are referenced with absolute paths
- Tables use standard markdown syntax with alignment

## Navigation Updates

When adding new pages:

1. Create the `.mdx` file in the appropriate directory
2. Add the page path to `docs.json` in the correct group
3. Consider adding "What's Next?" cards linking to the new page from related pages

## Testing Changes

Before committing:

1. Run the dev server and verify pages render correctly
2. Check that navigation shows the page in the right location
3. Test all code examples are syntactically valid
4. Verify links to other pages work

## Themes

Three themes available: `jam` (default), `nebula`, `pulsar`

## Deployment

Changes deploy automatically when pushed to the connected GitHub branch. The Jamdesk build service compiles MDX to HTML and deploys to CDN.

To trigger a rebuild manually:

1. Go to dashboard project settings
2. Click "Rebuild"

---
*Last reviewed: 2026-01-12*
