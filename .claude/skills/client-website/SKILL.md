---
name: client-website
description: End-to-end workflow for building a brand-new website for a business from scratch. Runs intake → brand book → website build → free Vercel deploy → returns a live link. Use whenever the user says they want a website for a business they are starting or running, even if no name, logo, or colours exist yet. Triggers on "build me a website", "I want a website", "make a website for my new business", "website for [business name]", "I'm starting a new business and need a site", "can you make me a site", "client-website".
---

# client-website

You are about to build a real, deployable website for a real business. This is not a mockup, not a wireframe, not a draft. By the end of this workflow, the user has a live URL they can hand to anyone.

You build it carefully, in stages, with the user's approval at each pause. You never invent brand details. You never publish without showing them first.

## What the user needs to know upfront (say this in your first reply)

When the workflow is triggered, your first message should land these four points in your own voice, lowercase, no AI-speak:

1. **They don't need to buy a domain.** You will publish the site to a free Vercel address using their business name (e.g. `business-name.vercel.app`). They can add a custom domain later if they want — not required to get live.
2. **You will build a brand book first.** Before any website work, you make a 1-page brand book PDF + a design system reference. This is what stops the site looking like AI slop.
3. **If they don't have a name / logo / colours / fonts, that's fine.** You can research and propose options for any of those they're missing. They approve before you proceed.
4. **They will see and approve everything.** Brand book first. Then website. Nothing publishes without their okay.

Then ask the intake questions below.

---

## Phase 1: Intake

Ask these questions one at a time or as a short numbered list — whichever feels natural in the conversation. Don't dump all 8 at once in a corporate form.

1. **Business name** — do you have one? If not, what does the business do — I can suggest names.
2. **What does the business actually do** — one or two sentences, in your words. Not a tagline yet, just the truth of it.
3. **Who is it for** — who's the customer / client / person who walks in or books?
4. **Logo** — do you have one? (yes / no / "I have a rough idea")
5. **Colours** — do you have a palette? (yes / no / "I like X kind of feel")
6. **Fonts** — do you have specific fonts you want? (yes / no / usually no)
7. **Existing site or socials** — anywhere I can look at to understand the vibe? (Instagram, an old site, a doc you've written)
8. **The single most important thing a visitor should feel or learn in the first 10 seconds** — this is the brief that drives everything downstream. Help them write it if they're stuck.

If they answer "no" to logo / colours / fonts: explicitly offer to research and propose options before moving on. Don't barrel ahead with defaults. Say something like:

> "no worries — I can pull together 2-3 colour palette options and 2-3 font pairings that match the brief, plus a simple logo concept. takes me about 5 minutes. want me to do that before we lock the brand book?"

If they say yes, run **Phase 1a: Brand research**. If they say "just pick", run **Phase 1b: Decisive picks**.

### Phase 1a: Brand research (only if missing assets)

For colour palettes:
- Pull 2-3 distinct directions matched to the brief. One safe, one bolder, one editorial. Don't show 10 — that's just stress.
- For each, give: 3-4 hex codes, a one-sentence description of feel, and what kind of business this palette suits. No long explanations.

For fonts:
- Pull 2-3 Google Fonts pairings (display + body). Reference the existing `site-redesign` skill's font pairings (F1-F10) for inspiration but don't be limited to them.
- Show each as: name + one-line mood ("Outfit + Cormorant Garamond italic — elegant, restrained, photo-led").

For a logo:
- Only generate a logo if the user wants one. If they say "I'll get a logo later", just plan the site without one and use a wordmark from the chosen font pairing.
- If yes: use the `gpt-image-2` skill to generate 2-3 logo concept options. Prompt should describe the business and the feel from question 8, NOT just say "logo". Keep them simple, single colour or two colour, scalable.

Present all options to the user, let them pick, then move to Phase 1b.

### Phase 1b: Decisive picks

Lock the final values:
- Business name
- Tagline (write one if they don't have one — keep it short, real, no buzzwords)
- Primary / secondary / tertiary hex codes
- Font pairing (display + body Google Fonts)
- Logo (file path or "wordmark only")
- The 10-second brief

State these back in a short numbered summary. Wait for approval before Phase 2.

---

## Phase 2: Brand book

Run the `design-system` skill to produce:

1. **`design-system.html`** — scrollable reference page (colours, type, principles, components, wordmarks).
2. **`brand-book-a4.html` → `brand-book-a4.pdf`** — the 1-page A4 portrait brand book PDF.

Then also write a **`DESIGN.md`** using the `designmd` skill format. This is the machine-readable brand spec the website build will read. Save it at the project root.

Show the user:
- The 1-page brand book PDF (it's the easiest thing to look at and approve)
- The `design-system.html` if they want to scroll the full thing

Ask: "happy with this? want any changes to colours, fonts, or the wordmark before I build the site?"

Iterate on feedback. Don't move to Phase 3 until they sign off on the brand book.

---

## Phase 3: Website build

This is where the "no AI slop" rules from the `site-redesign` skill apply hardest. Read that skill in full before generating anything.

### Hard rules — non-negotiable

1. **No common AI-default fonts.** No Inter alone, no Roboto, no plain Arial. Use the font pairing locked in Phase 1b. Display font must be a real serif or distinctive sans, paired with a body font that complements it.
2. **No generic stock photo people grinning at laptops.** Use real photography that fits the business. If you can't find good photography, lean on typography, generous space, and editorial layout instead of filler stock.
3. **Pick a hero pattern (HP-1 to HP-8) and section composition (A to G) from `site-redesign`** based on what this specific business actually is. Don't default to "image right + text left + 3 service cards". Read the 10-second brief and let it drive the choice.
4. **Pick a motion grammar level (M-1 to M-4)** that matches the brief. A calm brand gets stillness, not kinetic typography.
5. **Single file.** `index.html` with all CSS in `<style>` and all JS in `<script>`. Google Fonts via CDN. GSAP for animation. Lucide for icons.
6. **Real copy.** No `[Lorem ipsum]`. No `[Your service here]`. Every line of copy is real, written for this business, in their voice.
7. **AU English in all copy.** Colour, organise, centre, behaviour. No em dashes (—) or en dashes (–) anywhere. Use commas, full stops, parentheses, or "and"/"but".
8. **Reads the `DESIGN.md` tokens.** Colours, fonts, spacing, rounded values come from DESIGN.md, not from re-invented numbers.
9. **Mobile and desktop both look intentional.** Test mentally at 375px and 1440px. Don't ship a desktop-only design with a broken mobile fallback.
10. **One CTA, primary.** Don't paint the site with 14 different buttons.

### Build process

1. Read `DESIGN.md` from the project root for tokens.
2. Read `site-design-references` skill — pick 2-3 structural patterns appropriate to the sector. NEVER lift colour, font, or copy from references. Structural inspiration only. Never name the reference in the output.
3. Read the `site-redesign` skill — use its hero patterns, compositions, motion grammar, and quality standards (GSAP animations, noise overlay, magnetic buttons, responsive breakpoints).
4. Decide hero pattern + composition + motion level from the 10-second brief.
5. Write `index.html` at `sites/{business-slug}/index.html`. Slug is lowercase, hyphenated.
6. Preview it locally before showing the user — open in a browser, scroll through, check that nothing is broken or invisible.

### Pause and show

Show the user the local preview before deploying. Either:
- Open the file in their browser (local), or
- Start `python3 -m http.server 8080` from the `sites/` folder and give them `http://localhost:8080/{slug}/` (remote/SSH)

Ask: "want any tweaks before I deploy this live?"

If they want small visual adjustments (spacing, radius, font size, colour intensity), use the `tweak-html` skill — it injects a live slider panel into the page so they can dial it in themselves, then you bake the values back.

Iterate. Do not deploy until they say "looks good, ship it" or equivalent.

---

## Phase 4: Deploy to Vercel

Use the `vercel-deploy` skill.

1. Verify Vercel CLI is installed. If not, run `npm install -g vercel`.
2. Verify auth. If first run, the user runs `vercel login` once in their terminal.
3. Deploy: `vercel deploy --yes --prod sites/{slug}`
4. Capture the live URL — should look like `https://{slug}.vercel.app`.
5. Verify the URL loads (curl returns 200) before declaring done.

### What to send back to the user

A short, lowercase, no-AI-slop message with:
- The live URL (clickable)
- A reminder that the Vercel address is free and theirs forever, and they can later add a custom domain at vercel.com if they want
- The path to the local files (so they have the source)
- A line offering to make further changes anytime

Example tone:
> "live: https://sharni-co.vercel.app
>
> that's the free vercel address — yours, no hosting bill ever. if you want a custom .com later, vercel lets you point one at this site in about 2 minutes. files are at sites/sharni-co/ if you ever want a copy.
>
> want me to tweak anything?"

---

## What this workflow is NOT

- Not a CMS. The site is static HTML. Edits go through Claude.
- Not a blog platform out of the gate. If the business needs a blog, that's a separate conversation about whether to add Notion-backed content, a separate Vercel project, or move to a different stack.
- Not a lead scraping or competitor research pipeline. This is a single business building a single site for themselves. (For that, the `beautiful-websites` pipeline is a different beast.)
- Not designed for e-commerce. If the user wants to sell products online, flag that and discuss whether a static site with Stripe payment links is enough, or whether they need a real store (Shopify, etc).

If the user asks for any of those, address it explicitly. Don't quietly try to bolt them on.

---

## Skill chaining map

This skill orchestrates these other skills in the kit:

| Phase | Skill called | Purpose |
|-------|--------------|---------|
| 1a | `gpt-image-2` | Generate logo concepts if user has no logo |
| 2 | `design-system` | Generate brand book PDF + design system reference |
| 2 | `designmd` | Write DESIGN.md machine-readable spec |
| 3 | `site-redesign` | Use its rules library (hero patterns, fonts, palettes, motion, quality standards) for the build |
| 3 | `site-design-references` | Silent structural inspiration |
| 3 | `tweak-html` | Live tuning panel for post-build adjustments |
| 4 | `vercel-deploy` | Free deploy to .vercel.app |

All of these are bundled in this kit at `.claude/skills/`.

---

## House rules (everywhere in output)

- **AU English** — colour, organise, centre, behaviour, optimise, realise
- **No em dashes or en dashes** anywhere. Period.
- **Lowercase, conversational** in messages to the user. No "I'd be delighted to help you craft..." — that's AI slop and the user will smell it.
- **Real over generic.** If you can't write something real, ask the user a question instead of making something up.
- **Never publish without explicit approval.** Show first. Always.
