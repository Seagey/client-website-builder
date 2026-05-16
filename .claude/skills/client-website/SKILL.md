---
name: client-website
description: End-to-end workflow for building a brand-new website for a business from scratch. Runs intake, brand book, website build, free Vercel deploy, and returns a live link. Use whenever the user says they want a website for a business they are starting or running, even if no name, logo, or colours exist yet. Triggers on "build me a website", "I want a website", "make a website for my new business", "website for [business name]", "I'm starting a new business and need a site", "can you make me a site", "client-website".
---

# client-website

This is the orchestrator skill for the kit. It runs the full website-build workflow end to end: intake, brand research, brand book, site build, polish, and free Vercel deploy. By the end of the workflow, the user has a live URL they can hand to anyone.

It is not a mockup or a wireframe. It builds and ships a real site. It also never publishes anything without showing the user first and waiting for explicit approval.

## What the user needs to know upfront

When the workflow is triggered, your first reply should land these four points clearly. Use normal sentence case. Do not use em dashes or en dashes.

1. **They don't need to buy a domain.** You will publish the site to a free Vercel address based on their business name (e.g. `business-name.vercel.app`). They can add a custom domain later if they want. Not required to get live.
2. **You will build a brand book first.** Before any website work, you make a 1-page brand book PDF plus a design system reference. This is what stops the site looking like AI slop.
3. **If they don't have a name, logo, colours, or fonts, that's fine.** You can research and propose options for any of those they're missing. They approve before you proceed.
4. **They will see and approve everything.** Brand book first. Then website. Nothing publishes without their okay.

Then ask the intake questions below.

---

## Phase 1: Intake

Ask these questions one at a time or as a short numbered list, whichever feels natural in the conversation. Do not dump all 8 at once in a corporate form.

1. **Business name.** Do you have one? If not, what does the business do, I can suggest names.
2. **What does the business actually do.** One or two sentences, in your words. Not a tagline yet, just the truth of it.
3. **Who is it for.** Who's the customer, client, or person who walks in or books?
4. **Logo.** Do you have one? (Yes, no, or "I have a rough idea")
5. **Colours.** Do you have a palette? (Yes, no, or "I like X kind of feel")
6. **Fonts.** Do you have specific fonts you want? (Usually no, that's fine)
7. **Existing site or socials.** Anywhere I can look at to understand the vibe? (Instagram, an old site, a doc you've written)
8. **The single most important thing a visitor should feel or learn in the first 10 seconds.** This is the brief that drives everything downstream. Help them write it if they're stuck.

If they answer "no" to logo, colours, or fonts: explicitly offer to research and propose options before moving on. Do not barrel ahead with defaults. Something like:

> "No worries, I can pull together 2 to 3 colour palette options and 2 to 3 font pairings that match the brief, plus a simple logo concept. Takes about 5 minutes. Want me to do that before we lock the brand book?"

If they say yes, run **Phase 1a: Brand research**. If they say "just pick", run **Phase 1b: Decisive picks**.

### Phase 1a: Brand research (only if missing assets)

For colour palettes:
- Pull 2 to 3 distinct directions matched to the brief. One safe, one bolder, one editorial. Do not show 10. That is just stress.
- For each, give 3 to 4 hex codes, a one-sentence description of feel, and what kind of business this palette suits. No long explanations.

For fonts:
- Pull 2 to 3 Google Fonts pairings (display and body). The `site-builder` skill's font pairings (F1 to F10) are a good starting reference, but not the only options.
- Show each as: name plus one-line mood ("Outfit plus Cormorant Garamond italic, elegant, restrained, photo-led").

For a logo:
- Only generate a logo if the user wants one. If they say "I'll get a logo later", plan the site without one and use a wordmark from the chosen font pairing.
- If yes, use the `gpt-image-2` skill to generate 2 to 3 logo concept options. The prompt should describe the business and the feel from question 8, not just say "logo". Keep them simple, single colour or two colour, scalable.

Present all options to the user, let them pick, then move to Phase 1b.

### Phase 1b: Decisive picks

Lock the final values:
- Business name
- Tagline (write one if they don't have one, keep it short, real, no buzzwords)
- Primary, secondary, tertiary hex codes
- Font pairing (display and body Google Fonts)
- Logo (file path or "wordmark only")
- The 10-second brief

State these back in a short numbered summary. Wait for approval before Phase 2.

---

## Phase 2: Brand book

Run the `design-system` skill in **Path B** (intake notes mode). Pass it the locked values from Phase 1b: business name, tagline, primary/secondary/tertiary hex codes, font pairing, logo decision. The skill renders these into:

1. **`design/design-system.html`**. Scrollable reference page (colours, type, principles, components, wordmarks).
2. **`design/brand-book-a4.pdf`** (rendered from `design/brand-book-a4.html`). The 1-page A4 portrait brand book PDF.

Then write a **`DESIGN.md`** at the project root using the `designmd` skill format. This is the machine-readable brand spec the website build will read. The DESIGN.md uses the same locked values, structured as YAML tokens.

Important: design-system's Path B explicitly authors from intake notes rather than extracting from an external reference. The locked values ARE the reference. This is not "inventing" because the user already chose and approved each value during Phase 1b.

Show the user:
- The 1-page brand book PDF (it's the easiest thing to look at and approve)
- The `design-system.html` if they want to scroll the full thing

Ask: "Happy with this? Want any changes to colours, fonts, or the wordmark before I build the site?"

Iterate on feedback. Do not move to Phase 3 until they sign off on the brand book.

---

## Phase 3: Website build

This is where the "no AI slop" rules from the `site-builder` skill apply hardest. Read that skill in full before generating anything.

### Hard rules (non-negotiable)

1. **No common AI-default fonts.** No Inter alone, no Roboto, no plain Arial. Use the font pairing locked in Phase 1b. Display font must be a real serif or distinctive sans, paired with a body font that complements it.
2. **No generic stock photo people grinning at laptops.** Use real photography that fits the business. If you can't find good photography, lean on typography, generous space, and editorial layout instead of filler stock.
3. **Pick a hero pattern (HP-1 to HP-8) and section composition (A to G) from `site-builder`** based on what this specific business actually is. Do not default to "image right plus text left plus 3 service cards". Read the 10-second brief and let it drive the choice.
4. **Pick a motion grammar level (M-1 to M-4)** that matches the brief. A calm brand gets stillness, not kinetic typography.
5. **Single file.** `index.html` with all CSS in `<style>` and all JS in `<script>`. Google Fonts via CDN. GSAP for animation. Lucide for icons.
6. **Real copy.** No `[Lorem ipsum]`. No `[Your service here]`. Every line of copy is real, written for this business, in their voice.
7. **AU English in all copy.** Colour, organise, centre, behaviour. No em dashes (—) or en dashes (–) anywhere. Use commas, full stops, parentheses, or "and" or "but".
8. **Reads the `DESIGN.md` tokens.** Colours, fonts, spacing, rounded values come from DESIGN.md, not from re-invented numbers.
9. **Mobile and desktop both look intentional.** Test mentally at 375px and 1440px. Do not ship a desktop-only design with a broken mobile fallback.
10. **One primary CTA.** Do not paint the site with 14 different buttons.

### Build process

1. Read `DESIGN.md` from the project root for tokens.
2. Read `site-design-references` skill, pick 2 to 3 structural patterns appropriate to the sector. Never lift colour, font, or copy from references. Structural inspiration only. Never name the reference in the output.
3. Read the `site-builder` skill, use its hero patterns, compositions, motion grammar, and quality standards (GSAP animations, noise overlay, magnetic buttons, responsive breakpoints).
4. Decide hero pattern, composition, and motion level from the 10-second brief.
5. Write `index.html` at `sites/{business-slug}/index.html`. Slug is lowercase, hyphenated.
6. Preview it locally before showing the user. Open in a browser, scroll through, check that nothing is broken or invisible.

### Pause and show

Show the user the local preview before deploying. Either:
- Open the file in their browser, or
- Start `python3 -m http.server 8080` from the `sites/` folder and give them `http://localhost:8080/{slug}/`

Ask: "Want any tweaks before I deploy this live?"

If they want small visual adjustments (spacing, radius, font size, colour intensity), use the `tweak-html` skill. It injects a live slider panel into the page so they can dial it in themselves, then you bake the values back.

Iterate. Do not deploy until they say "looks good, ship it" or equivalent.

---

## Phase 4: Deploy to Vercel

Use the `vercel-deploy` skill.

1. Verify Vercel CLI is installed. If not, the user runs `npm install -g vercel`.
2. Verify auth. If first run, the user runs `vercel login` once in their terminal.
3. Deploy: `vercel deploy --yes --prod sites/{slug}`.
4. Capture the live URL. Should look like `https://{slug}.vercel.app`.
5. Verify the URL loads (curl returns 200) before declaring done.

### What to send back to the user

A short message with:
- The live URL (clickable)
- A reminder that the Vercel address is free and theirs forever, and they can later add a custom domain at vercel.com if they want
- The path to the local files (so they have the source)
- A line offering to make further changes anytime

Example tone:

> "Live at https://sharni-co.vercel.app.
>
> That's the free Vercel address. Yours, no hosting bill ever. If you want a custom .com later, Vercel lets you point one at this site in about 2 minutes. Files are at `sites/sharni-co/` if you ever want a copy.
>
> Want me to tweak anything?"

---

## What this workflow is NOT

- Not a CMS. The site is static HTML. Edits go through Claude.
- Not a blog platform out of the gate. If the business needs a blog, that's a separate conversation about whether to add Notion-backed content, a separate Vercel project, or move to a different stack.
- Not a lead scraping or competitor research pipeline. This is a single business building a single site for themselves.
- Not designed for e-commerce. If the user wants to sell products online, flag that and discuss whether a static site with Stripe payment links is enough, or whether they need a real store (Shopify, etc).

If the user asks for any of those, address it explicitly. Do not quietly try to bolt them on.

---

## Skill chaining map

This skill orchestrates these other skills in the kit:

| Phase | Skill called | Purpose |
|-------|--------------|---------|
| 1a | `gpt-image-2` | Generate logo concepts if user has no logo |
| 2 | `design-system` | Generate brand book PDF and design system reference |
| 2 | `designmd` | Write DESIGN.md machine-readable spec |
| 3 | `site-builder` | Use its rules library (hero patterns, fonts, palettes, motion, quality standards) for the build |
| 3 | `site-design-references` | Silent structural inspiration |
| 3 | `tweak-html` | Live tuning panel for post-build adjustments |
| 4 | `vercel-deploy` | Free deploy to .vercel.app |

All of these are bundled in this kit at `.claude/skills/`.

---

## House rules (everywhere in output)

- **AU English.** Colour, organise, centre, behaviour, optimise, realise.
- **No em dashes (—) or en dashes (–)** anywhere. Australian English doesn't use them. Use commas, full stops, parentheses, or "and" or "but" instead.
- **Sentence case, conversational.** Normal English capitalisation. Sharp and direct, not corporate filler. Avoid AI-flavoured copy ("I'd be delighted to assist you in crafting..."), the user will smell it.
- **Real over generic.** If you can't write something real, ask the user a question instead of making something up.
- **Never publish without explicit approval.** Show first. Always.
