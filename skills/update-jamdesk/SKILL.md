---
name: update-jamdesk
description: Update your Jamdesk documentation when code changes affect user-facing features. Use when user says "update docs", "document this", "add to jamdesk docs", or after implementing user-facing changes.
user_invocable: true
---

# Update Jamdesk

## Overview

Updates your Jamdesk documentation when code changes affect user-facing features. This skill:
- Targets documentation pages (not CLAUDE.md)
- Locates docs via `.jamdesk-docs-path` config file or `./docs` directory
- Asks clarifying questions about scope and approach
- Creates descriptive, well-written documentation
- Verifies with jamdesk CLI (if available)

**Announce at start:** "I'm using the update-jamdesk skill to update your documentation."

**When to use:**
- After implementing user-facing features
- When user explicitly asks to document something
- After changing CLI commands, APIs, or component behavior

## Flags

| Flag | Description |
|------|-------------|
| `--preview` | Show what would be updated without making changes (dry-run) |

**Examples:**
- `/update-jamdesk` - Full workflow with questions and changes
- `/update-jamdesk --preview` - Show analysis and proposed changes only

## Proactive Suggestion

After the `/commit` skill completes with user-facing file changes, suggest:

```
Tip: These changes affect user-facing features. Run `/update-jamdesk` to update your documentation.
```

User-facing patterns to detect:
- `**/api/**` - API endpoints
- `**/cli/**` or `**/commands/**` - CLI commands
- `**/components/**` - UI components
- Changes to public interfaces or types

## Preview Mode Handling

When `--preview` flag is used:
- Complete Phases 1-3 normally (locate, clarify, analyze)
- In Phase 4, **describe** what would be written but don't create/edit files
- Skip Phases 5-6 (verification and commit)
- End with: "Preview complete. Run `/update-jamdesk` without --preview to make these changes."

## Phase 1: Locate Documentation

### Default Locations

Check for docs in this order:

1. `.jamdesk-docs-path` config file (if exists)
2. `./docs` directory with `docs.json`
3. Current directory with `docs.json`
4. Ask user to specify path

```bash
# Check default locations
if [ -f ".jamdesk-docs-path" ]; then
  docs_path=$(cat .jamdesk-docs-path | grep -E "^docs_path:" | cut -d: -f2 | tr -d ' ')
elif [ -f "./docs/docs.json" ]; then
  docs_path="./docs"
elif [ -f "./docs.json" ]; then
  docs_path="."
fi
```

### Config File Format

If your docs are in a separate repository, create `.jamdesk-docs-path`:

**Simple format (path only):**
```
../my-docs
```

**Full format (YAML):**
```yaml
# Path to documentation repository
docs_path: ../my-docs

# Optional: Branch for doc updates (default: main)
docs_branch: main
```

### Validate Docs Path

After finding docs location:

1. Verify directory exists
2. Verify it contains a `docs.json` file
3. If invalid, ask user to correct

### Check for Uncommitted Changes

Before making changes, check for uncommitted work:

```bash
if [ -n "$(git status --porcelain)" ]; then
  echo "Warning: You have uncommitted changes."
  echo "Would you like to:"
  echo "1. Continue anyway (changes will be mixed)"
  echo "2. Stash changes first"
  echo "3. Cancel and let me commit first"
fi
```

## Phase 2: Gather Context and Clarify

### Analyze Code Changes

Review the current conversation to identify:
- Which files were modified
- What feature/behavior changed
- Whether changes are user-facing

### User-Facing Detection

A change is **user-facing** if it affects:

| Change Type | Examples | Docs Impact |
|-------------|----------|-------------|
| API changes | New endpoints, changed responses | API reference |
| CLI commands | New flags, changed behavior | CLI docs |
| UI changes | New buttons, changed flows | User guides |
| Config options | New settings, changed defaults | Configuration docs |
| Component props | New props, changed behavior | Component docs |

### Clarifying Questions

**Always ask these questions before proceeding:**

#### 1. Branch Strategy

```
How should I handle changes?

1. Create a new feature branch (recommended for review)
2. Update directly on the current branch
```

#### 2. Scope Confirmation

```
Based on the changes, I plan to:
- [Update/Create]: [page name] - [brief description]

Is this correct? Any pages I should add or skip?
```

### Key Principle

**Ask first, write later.** Don't assume you understand the user's intent. A 30-second clarification prevents 10 minutes of rework.

## Phase 3: Analyze Existing Documentation

### Search for Related Content

Before writing, check what already exists:

```bash
# Search for existing coverage
grep -r "feature-keyword" --include="*.mdx" .

# Check navigation structure
cat docs.json | grep -A5 -B5 "feature-keyword"
```

### Classification

For each topic that needs documentation:

| Status | Action |
|--------|--------|
| **Not documented** | Create new page |
| **Partially documented** | Update existing page |
| **Fully documented** | Report no changes needed |
| **Outdated** | Update with new information |

### Output

Present findings to user:

```
## Documentation Analysis

### Existing Coverage
- `getting-started.mdx` - Mentions feature briefly in intro
- `api-reference.mdx` - No coverage

### Recommended Changes
1. **Update**: `getting-started.mdx` - Add link to new detailed page
2. **Create**: `features/new-feature.mdx` - Full documentation

Does this approach look right?
```

## Phase 4: Write Documentation

### Writing Standards

Follow the jamdesk-docs writing standards from https://jamdesk.com/docs:

**Structure every page with:**
1. Opening paragraph - what and why
2. Quick example - simplest working code
3. Detailed explanation - comprehensive coverage
4. What's Next - related pages

**Writing style:**
- Active voice ("Run this command" not "This command should be run")
- Progressive disclosure (simple first, advanced later)
- Complete, runnable code examples
- One idea per paragraph

### Page Template

```mdx
---
title: Feature Name
description: Brief description for SEO (50-160 chars)
---

Opening paragraph explaining what this feature does and why it's useful.

## Quick Start

The simplest example to get started:

\`\`\`bash
command --example
\`\`\`

## Configuration

<ParamField name="optionName" type="string" required>
  Description of what this option does.
</ParamField>

## Examples

### Basic Usage

\`\`\`typescript
// Complete, runnable example
\`\`\`

## What's Next?

<CardGroup cols={2}>
  <Card title="Related Feature" icon="icon-name" href="/path">
    Brief description
  </Card>
</CardGroup>
```

### Component Usage

| Component | Use Case |
|-----------|----------|
| `<Tabs>` | Mutually exclusive options (npm/yarn, languages) |
| `<Steps>` | Sequential procedures |
| `<Accordion>` | Optional/advanced content |
| `<Cards>` | Navigation and feature highlights |
| `<Note>`, `<Warning>`, `<Tip>` | Important callouts (use sparingly) |
| `<CodeGroup>` | Same code in multiple languages |

### Navigation Updates

When creating new pages, update `docs.json`:

```json
{
  "navigation": {
    "tabs": [{
      "groups": [{
        "group": "Group Name",
        "pages": ["existing-page", "new-page"]
      }]
    }]
  }
}
```

## Phase 5: Verify Documentation

### Jamdesk CLI Verification

If the jamdesk CLI is available, run verification:

```bash
# Validate docs.json schema
jamdesk validate

# Check for broken links
jamdesk broken-links
```

**Expected output:**
```
✓ docs.json is valid
✓ No broken links found
```

### Installing the CLI

If `jamdesk` command not found:

```bash
npm install -g jamdesk
```

Or skip automated verification and use manual checks.

### Manual Verification Checklist

Even without CLI, verify:
- [ ] All code examples have language tags
- [ ] Frontmatter has title and description
- [ ] Links use correct paths (no .mdx extension)
- [ ] Images use absolute paths from root
- [ ] New pages added to docs.json navigation

### If Verification Fails

**Broken links found:**
```
jamdesk broken-links found issues:
- docs/feature.mdx:15 - /docs/nonexistent

Would you like me to fix these? [Y/n]
```

## Phase 6: Commit and Complete

### Present Changes

Before committing, show summary:

```
## Documentation Updates Complete

### Files Changed
- Created: `features/new-feature.mdx`
- Updated: `getting-started.mdx` (added link)
- Updated: `docs.json` (added to navigation)

### Verification
✓ jamdesk validate passed
✓ jamdesk broken-links passed

### Next Steps
Would you like me to:
1. Commit these changes
2. Show the full diff first
3. Make additional changes
```

### Git Operations

```bash
# If creating feature branch (as agreed in Phase 2)
git checkout -b docs/feature-name

# Stage and commit
git add .
git commit -m "docs: add documentation for [feature]"
```

### Commit Message Format

```
docs: [action] [what was documented]

- [Specific change 1]
- [Specific change 2]
```

Examples:
- `docs: add API authentication guide`
- `docs: update CLI reference with new flags`
- `docs: create webhook configuration page`

## Quick Reference

| Situation | Action |
|-----------|--------|
| No config file | Check ./docs, then ask user |
| Docs already exist | Update existing page |
| New feature | Create new page + update navigation |
| CLI not installed | Offer to install or skip verification |
| Broken links found | Fix automatically or ask user |
| `--preview` flag | Show analysis only, no changes |

## Red Flags

**Never:**
- Create stub documentation ("TODO: add content")
- Skip verification steps
- Write documentation without asking clarifying questions first
- Commit directly to main without asking
- Make changes when `--preview` flag is used

**Always:**
- Ask clarifying questions before writing
- Verify with jamdesk CLI when available
- Show changes before committing
- Follow jamdesk-docs writing standards
- Check for uncommitted changes first
