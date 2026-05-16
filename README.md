# Client Website Builder Kit

A Claude Code skill pack that gives any agent the ability to build a real, deployable website for a real business — intake, brand book, website, free Vercel deploy — without producing the usual AI-slop template look.

Built for handing to a Claude Code agent that doesn't currently know how to build websites. Drop it in, install, and the next time someone says "I want a website for my new business" the agent runs a careful 4-phase workflow instead of saying it can't do that.

---

## What it does

When the user says something like "I want a website for [business]" or "build me a website", the orchestrator skill (`client-website`) kicks in and runs:

1. **Intake.** Asks 8 questions about the business — name, what it does, audience, logo / colours / fonts, existing assets, and most importantly the 10-second brief (what a visitor needs to feel/learn in the first 10 seconds).
2. **Brand research (only if needed).** If the user doesn't have a logo, colours, or fonts, the agent offers to propose 2-3 directions for each. Logos generated with GPT Image 2. Palettes and fonts curated from a vetted library.
3. **Brand book.** Generates a 1-page A4 PDF brand book + a scrollable design-system reference page + a machine-readable `DESIGN.md`. User approves before any site work.
4. **Website build.** Single-file `index.html` site following strict no-AI-slop rules (no Inter alone, no Roboto, no stock-photo-people-grinning-at-laptops, no generic 3-card layout). Picks a hero pattern, composition, and motion grammar driven by the brief, not a default template.
5. **Polish.** Optional live slider panel (`tweak-html`) for the user to dial in spacing / colour / radius themselves before publishing.
6. **Deploy.** Pushes to Vercel free tier. Returns a `business-name.vercel.app` URL. No hosting bill, ever. Custom domain optional later.

Hard rule across everything: AU English, no em dashes, lowercase conversational tone, real copy, no placeholders.

---

## What's in the kit

```
.claude/skills/
├── client-website/         # The orchestrator. Runs the 4-phase workflow.
├── designmd/               # Authors the machine-readable DESIGN.md brand spec.
├── design-system/          # Generates the 1-page A4 brand book PDF + reference page.
├── site-redesign/          # Library of palettes, fonts, layouts, hero patterns, motion grammars, quality standards.
├── site-design-references/ # Curated structural inspiration (Airbnb, Apple, BMW, etc) — never lifted, only studied.
├── gpt-image-2/            # Logo generation via OpenAI's GPT Image 2 (Fal AI).
├── tweak-html/             # Drop-in live tuning panel for post-build adjustments.
└── vercel-deploy/          # Free deploy to .vercel.app.
```

`client-website` is the entry point. The others are called by it as needed. They are also runnable standalone if the agent wants to do just a brand book or just a deploy.

---

## Install

```bash
# Clone wherever your Claude Code workspace lives
git clone <repo-url> sharni-website-builder

# Copy the skills into your workspace's .claude/skills/ folder
cp -r sharni-website-builder/.claude/skills/* /path/to/your/workspace/.claude/skills/

# Or, if you want them per-project, drop them into a specific project's .claude/skills/ instead
```

Skills are auto-loaded by Claude Code on next session. No build step.

---

## Prerequisites

The agent needs these to be installed / available:

- **Node + npm** — for the Vercel CLI (`npm install -g vercel`)
- **Vercel account** — free Hobby tier is enough. First-time use: agent will prompt the user to run `vercel login` once.
- **A FAL_KEY in `.env`** (only if logo generation is used) — get one at [fal.ai](https://fal.ai). Cost is ~$0.05 per logo concept on the default medium quality setting.
- **A web browser on the user's machine** — for previewing sites before deploy, and for the brand book PDF render step (uses headless Edge / Chrome).
- **`gh` CLI** (optional, only if the agent will push the site source to GitHub on the user's behalf)

The agent will check for each and prompt the user to install / authenticate before it gets stuck.

---

## Cost

- Brand book + website build: free (runs locally).
- Logo generation (optional): ~$0.05 per concept, charged to whatever FAL_KEY is in `.env`. Skip the logo phase entirely if cost-sensitive.
- Hosting: free, forever. Vercel Hobby tier. Unlimited static sites.
- Custom domain (optional): user buys directly from a registrar, then points it at Vercel. Vercel doesn't charge for the domain itself.

---

## When to use this kit vs. the other website skills

If your agent already has the broader `beautiful-websites` pipeline installed (which does lead scraping → site qualifying → bulk site generation for outreach campaigns), this kit is the **single-business inverse**. It assumes:

- The user IS the business owner (not a marketer hunting for leads).
- They want one site, for one business.
- They want input on the brand direction, not just a redesign of an existing site.

Use `beautiful-websites` for outbound. Use this kit (`client-website`) for inbound — a client / friend / family member / yourself saying "I want a website."

---

## Workflow at a glance

```
user: "I want a website for my new yoga studio"
  └─→ client-website skill triggers
       ├── Phase 1: Intake (asks 8 questions, captures brief)
       │    └── if missing logo/colours/fonts: offers Phase 1a brand research
       ├── Phase 2: Brand book (design-system + designmd)
       │    └── user approves PDF before moving on
       ├── Phase 3: Website build (site-redesign rules + DESIGN.md tokens)
       │    └── local preview, optional tweak-html polish
       └── Phase 4: Deploy (vercel-deploy)
            └── returns https://yoga-studio.vercel.app
```

Every phase pauses for user approval before the next one runs.

---

## House rules (baked into every skill in this kit)

- **AU English** — colour, organise, centre, behaviour, optimise, realise
- **No em dashes (—) or en dashes (–)** anywhere in output. Use commas, full stops, parentheses, or "and"/"but".
- **Lowercase, conversational** when writing copy or replies. No "I'd be delighted to help you craft..."
- **No invented brand details.** If a brand spec doesn't have colours / fonts / a logo, ask the user or offer research. Never make them up.
- **Never publish without approval.** Brand book → user signs off → site build → user signs off → deploy. No skipping.

---

## Credits

Adapted from skills in the Underseage agent workspace. Heavy lifting:

- `design-system` is a refit of Google's open `design.md` spec for one-page brand books.
- `site-redesign` palette / font / layout / hero pattern libraries are from the Underseage Beautiful Websites pipeline.
- `gpt-image-2` is OpenAI's image model, accessed via Fal AI.
- `vercel-deploy` uses the official Vercel CLI on the free Hobby tier.

This kit packages them into a single "build a website from scratch" workflow for handing to agents that don't have them yet.
