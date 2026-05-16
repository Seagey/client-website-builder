# Client Website Builder Kit

A Claude Code skill pack that gives any agent the ability to build a real, deployable website for a real business: intake, brand book, website, free Vercel deploy. Without producing the usual AI-slop template look.

Built for handing to a Claude Code agent that doesn't currently know how to build websites. Drop it in, install, and the next time someone says "I want a website for my new business" the agent runs a careful 4-phase workflow instead of saying it can't do that.

---

## What it does

When the user says something like "I want a website for [business]" or "build me a website", the orchestrator skill (`client-website`) kicks in and runs:

1. **Intake.** Asks 8 questions about the business: name, what it does, audience, logo, colours, fonts, existing assets, and most importantly the 10-second brief (what a visitor needs to feel or learn in the first 10 seconds).
2. **Brand research (only if needed).** If the user doesn't have a logo, colours, or fonts, the agent offers to propose 2 to 3 directions for each. Logos generated via OpenAI's GPT Image 2 (Fal AI or Kie.ai). Palettes and fonts curated from a vetted library.
3. **Brand book.** Generates a 1-page A4 PDF brand book plus a scrollable design-system reference page plus a machine-readable `DESIGN.md`. User approves before any site work.
4. **Website build.** Single-file `index.html` site following strict no-AI-slop rules (no Inter alone, no Roboto, no stock-photo-people-grinning-at-laptops, no generic 3-card layout). Picks a hero pattern, composition, and motion grammar driven by the brief, not a default template.
5. **Polish.** Optional live slider panel (`tweak-html`) for the user to dial in spacing, colour, and radius themselves before publishing.
6. **Deploy.** Pushes to Vercel free tier. Returns a `business-name.vercel.app` URL. No hosting bill, ever. Custom domain optional later.

Hard rule across everything: AU English, no em dashes, no en dashes (AU English doesn't use them), sentence case, real copy, no placeholders.

---

## What's in the kit

```
.claude/skills/
├── client-website/         # The orchestrator. Runs the 4-phase workflow.
├── designmd/               # Authors the machine-readable DESIGN.md brand spec.
├── design-system/          # Generates the 1-page A4 brand book PDF plus reference page.
├── site-builder/           # Library of palettes, fonts, layouts, hero patterns, motion grammars, quality standards. From-scratch site generator.
├── site-design-references/ # Curated structural inspiration (Airbnb, Apple, BMW, etc). Never lifted, only studied.
├── gpt-image-2/            # Logo generation via OpenAI's GPT Image 2 (Fal AI preferred, Kie.ai fallback).
├── tweak-html/             # Drop-in live tuning panel for post-build adjustments.
└── vercel-deploy/          # Free deploy to .vercel.app.
```

`client-website` is the entry point. The others are called by it as needed. They are also runnable standalone if the agent wants to do just a brand book or just a deploy.

---

## Install

```bash
# Clone wherever your Claude Code workspace lives
git clone https://github.com/Seagey/client-website-builder

# Copy the skills into your workspace's .claude/skills/ folder
cp -r client-website-builder/.claude/skills/* /path/to/your/workspace/.claude/skills/

# Or, if you want them per-project, drop them into a specific project's .claude/skills/ instead
```

Skills are auto-loaded by Claude Code on next session. No build step.

---

## Prerequisites

The agent will check for each of these and prompt the user to install or authenticate before getting stuck. Nothing in the kit assumes you already have these set up.

- **Node and npm.** Needed for the Vercel CLI. Install from https://nodejs.org.
- **Vercel CLI.** `npm install -g vercel`. The agent will install it for the user if they haven't already.
- **Vercel account.** Free Hobby tier is enough. First-time use: agent will prompt the user to run `vercel login` once in their terminal.
- **Image generation key (optional, only if logo generation is used).** Two options:
  - **Fal AI** (recommended). Get a key at https://fal.ai, set as `FAL_KEY` in shell or project `.env`. ~5 cents USD per logo concept at medium quality.
  - **Kie.ai** (alternative). Get a key at https://kie.ai, set as `KIE_AI_KEY`. Same model, similar cost.
  - Skip logo generation entirely if cost-sensitive or no API key available. The agent can use a wordmark from the chosen font pairing instead.
- **A web browser on the user's machine.** For previewing sites before deploy, and for the brand book PDF render step (uses headless Edge or Chrome).

---

## Cost

- Brand book and website build: free. Runs locally on the user's machine.
- Logo generation (optional): ~5 cents USD per concept at medium quality, charged to whichever API key is configured. Skip entirely if cost-sensitive.
- Hosting: free, forever. Vercel Hobby tier. Unlimited static sites.
- Custom domain (optional): user buys directly from a registrar, then points it at Vercel. Vercel doesn't charge for the domain itself.

---

## When to use this kit vs. something else

This kit is built for the inbound case: a user (or someone the user is helping) wants ONE site for ONE business that they own or are starting.

It is not a lead-generation pipeline (does not scrape, qualify, or build sites in bulk for outreach). It is not a CMS, blog platform, or e-commerce engine. If the project needs any of those, the agent will flag it explicitly before going further.

---

## Workflow at a glance

```
user: "I want a website for my new yoga studio"
  └─→ client-website skill triggers
       ├── Phase 1: Intake (asks 8 questions, captures brief)
       │    └── if missing logo/colours/fonts: offers Phase 1a brand research
       ├── Phase 2: Brand book (design-system + designmd)
       │    └── user approves PDF before moving on
       ├── Phase 3: Website build (site-builder rules + DESIGN.md tokens)
       │    └── local preview, optional tweak-html polish
       └── Phase 4: Deploy (vercel-deploy)
            └── returns https://yoga-studio.vercel.app
```

Every phase pauses for user approval before the next one runs.

---

## House rules (baked into every skill in this kit)

- **AU English.** Colour, organise, centre, behaviour, optimise, realise.
- **No em dashes (—) or en dashes (–)** anywhere in output. Australian English doesn't use them. Use commas, full stops, parentheses, or "and" or "but" instead.
- **Sentence case, conversational.** Normal English capitalisation. No AI filler ("I'd be delighted to help you craft..."). Sharp and direct.
- **No invented brand details.** If a brand spec doesn't have colours, fonts, or a logo, ask the user or offer research. Never make them up.
- **Never publish without approval.** Brand book then user signs off, then site build then user signs off, then deploy. No skipping.

---

## Credits

- `design-system` is a refit of Google's open `design.md` spec for one-page brand books.
- `site-builder` palette, font, layout, hero pattern, composition, and motion grammar libraries.
- `gpt-image-2` is OpenAI's image model, accessed via Fal AI (preferred) or Kie.ai (fallback).
- `vercel-deploy` uses the official Vercel CLI on the free Hobby tier.

This kit packages them into a single "build a website from scratch" workflow for handing to agents that don't have them yet.
