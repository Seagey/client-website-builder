---
name: designmd
description: Author, lint, diff, and export DESIGN.md files. DESIGN.md is Google's open spec for describing a brand's visual identity to coding agents (yaml token front matter for colors, typography, rounded, spacing, components, plus markdown rationale prose). Use this skill whenever a project needs a machine-readable brand handoff doc, before site-builder or vercel-deploy work, or whenever a project needs a DESIGN.md at its root. Triggers on "designmd", "design.md", "write design tokens", "brand spec for agents", "lint design tokens", "export to tailwind theme".
---

# designmd

Single-file canonical brand spec for any project. The yaml at the top is what coding agents read; the markdown below is what humans read. One file, two audiences. Keep them aligned.

## When to use this skill

- A new project needs a `DESIGN.md` at its root.
- A brand engagement ships and needs a handoff doc for future agents.
- Before any `site-builder` or website generation run, so the worker has structured tokens, not vibes.
- Pre-deploy: lint to catch contrast failures and broken token refs before pushing live.
- Migrating a tailwind config: export rather than hand-maintain.

## Format reminder

```md
---
name: <Brand>
description: <one-line>
colors:
  primary: "#1A1C1E"
  secondary: "#6C7278"
  tertiary: "#B8422E"
  neutral: "#F7F5F2"
  on-primary: "#FFFFFF"
  on-tertiary: "#FFFFFF"
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 3rem
    fontWeight: 700
    lineHeight: 1.1
  body-md:
    fontFamily: Public Sans
    fontSize: 1rem
    lineHeight: 1.5
  label-caps:
    fontFamily: Space Grotesk
    fontSize: 0.75rem
    letterSpacing: 0.08em
rounded:
  sm: 4px
  md: 8px
  lg: 16px
spacing:
  sm: 8px
  md: 16px
  lg: 24px
components:
  button-primary:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.on-tertiary}"
    rounded: "{rounded.sm}"
    padding: 12px
  button-primary-hover:
    backgroundColor: "{colors.tertiary-container}"
---

## Overview
<one short paragraph: what this brand feels like, what surface it lives on>

## Colors
<bullets: each colour, hex, and what it's used for. NO em dashes>

## Typography
<which font for what, when to use the display vs body>

## Layout
<grid, max widths, breakpoints if relevant>

## Components
<one paragraph per component variant>

## Do's and Don'ts
<short list, opinionated>
```

Token reference syntax: `{colors.primary}` resolves at lint time. Components can reference colors, typography, rounded, spacing.

Section order is fixed: Overview → Colors → Typography → Layout → Elevation & Depth → Shapes → Components → Do's and Don'ts. Skip sections you don't need; don't reorder.

## How to author a DESIGN.md

1. **Find the brand source.** Existing brand book PDF, screenshots, hex codes in CSS, or notes captured during intake.
2. **Extract tokens.** Hex colours, font names and sizes, border-radius scale, spacing scale, component recipes.
3. **Write the yaml first.** Tokens are normative, get them right. Use referenced tokens (`{colors.tertiary}`) inside `components` instead of repeating hex values.
4. **Write the prose.** Lead each section with the WHY, not just the what. Two to four sentences per section is usually enough.
5. **House rules:** no em dashes or en dashes anywhere. Australian English (`colour`, `centre`, `optimise`). Sentence case, conversational tone in the markdown, not corporate filler.
6. **Lint before saving.** `npx @google/design.md lint DESIGN.md`. Exit code 1 means errors.

## CLI commands you'll actually use

```bash
# Lint a DESIGN.md (catches broken refs, contrast failures, structural errors)
npx @google/design.md lint DESIGN.md

# Diff two versions to flag regressions during reviews
npx @google/design.md diff DESIGN.md DESIGN-v2.md

# Export to Tailwind v4 theme block (CSS custom properties)
npx @google/design.md export --format css-tailwind DESIGN.md > theme.css

# Export to Tailwind v3 theme.extend object
npx @google/design.md export --format json-tailwind DESIGN.md > tailwind.theme.json

# Export to W3C Design Tokens (DTCG) for Figma / Style Dictionary
npx @google/design.md export --format dtcg DESIGN.md > tokens.json

# Pull the spec itself (useful for injecting into worker prompts)
npx @google/design.md spec
```

No npm install needed, `npx` resolves it on demand.

## Where DESIGN.md lives

- **Per project:** at the project root. The site-builder reads this when generating the website.
- **Brand engagements:** alongside the brand book PDF in `design/`. The 1-page A4 brand book is for humans, the DESIGN.md is for agents.

## Pre-deploy lint gate (optional)

Before deploying, check the spec is still valid:

```bash
if [ -f "DESIGN.md" ]; then
  npx @google/design.md lint "DESIGN.md" || exit 1
fi
```

Catches contrast regressions and broken token references before they ship.

## House rules when authoring DESIGN.md

- **No em or en dashes.** Replace with periods, commas, or "and"/"but". Hard rule across all output.
- **Australian English.** `colour`, `centre`, `behaviour`, `optimise`, `realise`.
- **Sentence case, conversational** in the markdown body. Sharp, not corporate.
- **Every colour token gets an `on-*` partner** (e.g. `on-primary`) so workers can pick a contrast-safe text colour without guessing.
- **Fonts:** prefer Google Fonts (free, agent can `<link>` them straight in). If the brand uses paid fonts, document the fallback stack.

## Common edge cases

- **Brand has no spec yet.** Run `design-system` skill first to extract from a reference, then convert its output into DESIGN.md form.
- **Project uses Tailwind already.** Generate DESIGN.md, then export to tailwind theme, then have the project consume the exported theme. Single source of truth lives in DESIGN.md.
- **Project uses no design system at all (vanilla CSS).** Still author DESIGN.md. Workers will reference it when adding components later.
- **Multiple brands in one project (parent + product + studio).** One DESIGN.md per brand, named `DESIGN-<brand>.md`. Document which is canonical in the project's CLAUDE.md.

## Output checklist

Before saying "done":
- [ ] yaml validates (`lint` returns exit 0)
- [ ] no em dashes anywhere
- [ ] AU English throughout
- [ ] each colour has an `on-*` partner where it's used as a background
- [ ] components reference tokens (`{colors.tertiary}`) not hardcoded hex
- [ ] section order respected (Overview → Colors → Typography → Layout → Elevation → Shapes → Components → Do's and Don'ts)
- [ ] saved to `<project-root>/DESIGN.md`
