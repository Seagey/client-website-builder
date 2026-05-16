---
name: site-redesign
description: Generate a premium single-file HTML/CSS/JS website redesign for a local business. Uses a mix-and-match design system with 10 palettes, 10 font pairings, and 5 layouts for unlimited unique combinations.
trigger: "site-redesign" or "redesign" or "build site"
---

# Skill: Site Redesign

## What This Skill Does
Takes a local business's existing website content and generates a single `index.html` file that looks like a $5,000+ custom design. No build step. No framework. Deploys instantly to Vercel.

---

## Installation

### Claude Code
Place this file at `.agent/skills/site-redesign/SKILL.md`. Claude Code auto-loads it.

### OpenClaw
Paste the contents of this file into your system prompt. Use the website prompt from `prompts/website_prompt_v1.md` as your generation template.

---

## How to Invoke

```
/site-redesign
```
Reads qualifying leads from `qualify_results.json` automatically and processes all `YES` entries.

Or for a single business:
```
/site-redesign https://zennailbar.com
```

---

## What the Agent Does

For each lead where `qualify === "YES"`:

1. Reads their current website content with `read_url_content`
2. **Writes a 3-line design brief for THIS business** (Step 0 below). Every design decision flows from this brief, not from a sector table. Two cafes can need radically different sites.
3. **Determines the SECTOR** as a starting filter only. Sector narrows the palette/font/layout subset (see "Sector-aware combo selection") but does NOT determine the structural shape of the site.
4. **Picks a hero pattern, section composition, and motion grammar** from the libraries below, driven by the design brief. This is what makes two wellness sites look genuinely different from each other instead of the-same-template-recoloured.
5. **Consults the `site-design-references` skill** for the sector. Use 2-3 structural patterns from the listed reference brands. NEVER lift colour, font, copy, or imagery — structural inspiration only. Never name the reference in the output.
6. **Checks `sites/build-log.md`** to avoid repeating a hero pattern + composition + motion combination on a similar business.
7. Picks palette + font + layout from the sector-aware subset.
8. **Picks photos with the photo identity rules** (see hard rule below). Verifies each URL loads (200 status) before using.
9. Generates a complete `index.html`.
10. Saves to `sites/{business-slug}/index.html`.
11. Logs to `sites/build-log.md` with all of: sector, hero pattern, composition, motion grammar, palette, font, layout, references used, and ONE-line summary of why this composition fits this business.

---

## Step 0: Write the design brief BEFORE anything else

Three sentences, mandatory, before opening any combo table.

> 1. **What this business is actually selling** (the human story, not the category — "thirty years of one-on-one quiet attention", not "massage therapy")
> 2. **Their single most distinctive differentiator** ("Joseph Heller-trained, 40-year unbroken Hellerwork lineage", not "experienced practitioner")
> 3. **What the visitor needs to feel/learn in the first 10 seconds** ("she has been doing this longer than they've been alive, and she sees them", not "book a massage")

Two cafes don't get the same brief. A 30-year solo practitioner and a multi-disciplinary clinic don't get the same brief even if both are "wellness". Write the brief by reading their actual content, looking at their existing imagery, and forming a real opinion about what's distinctive. If you can't write a non-generic brief, you don't understand the business well enough to design for them yet — go back and read more.

The brief drives EVERY downstream choice: hero pattern, composition, motion, photo treatment, even copy tone. If the brief says "stillness", motion is M-1 and the hero is HP-4. If the brief says "lineage", the composition is E and the hero is HP-6. Don't override these connections without an explicit reason.

---

## Hero patterns library (pick ONE per site, driven by the brief)

Don't default to "image right + text left". Pick the hero pattern that matches what this business IS.

| ID | Pattern | When to use | Don'ts |
|----|---------|-------------|--------|
| **HP-1** | **Text manifesto** — large typographic statement, no hero image, single CTA, generous space | Practitioners with a strong philosophy. Writers, consultants, designers, single-discipline therapists. When the words ARE the offer. | Don't use for visual-led businesses (florist, photographer). |
| **HP-2** | **Image-dominant** — full-bleed hero photo, small text overlay, restrained CTA | Businesses where visuals ARE the offer: photographers, florists, interior designers, restaurants with strong food photography, accommodation. | Don't use when you can't get strong on-brand photography. |
| **HP-3** | **Asymmetric split** — 60/40 text-and-image, single product/service framing | Service-as-product framing: a single hero offer (mechanic, premium dental, single-treatment clinic, signature cafe item). | Don't use for multi-service generalists. |
| **HP-4** | **Quote-first** — large pull-quote in serif takes the whole hero, photo and CTA delayed below the fold | Businesses sold on testimonial / reputation. Long-tenure practitioners. Heritage businesses. | Don't use when the business has no real testimonials. |
| **HP-5** | **Announcement** — the news IS the hero. Move, launch, transition, anniversary stated as the headline. | Businesses with a transition, launch, or single piece of news that is genuinely the most important thing for any visitor to know. | Don't use just to be different — needs real news. |
| **HP-6** | **Timeline** — hero is a horizontal/vertical timeline of dates that scroll-scrubs. | Businesses where heritage / lineage / years of practice IS the credibility (40-year practitioner, multi-generational family business, "since 1972"). | Don't use without real, specific dates. |
| **HP-7** | **Kinetic typography** — large oversized type that scroll-animates, marquee, scrubbed letter reveals. | Modern/youth/tech-adjacent brands where motion IS the brand. Bold trades that lean future-facing. | Don't use for calm/quiet/mature brands. |
| **HP-8** | **Live data / state** — hero is the current schedule, the live availability, the latest gallery item, today's specials. | Businesses where the current state IS the value: bookings-heavy salons, restaurants with daily menus, gyms with class schedules. | Requires real, fetchable data — don't fake it. |

**Rule:** within a sector, two consecutive sites in the build log MUST use different hero patterns. Re-roll if you'd repeat.

---

## Section composition library (pick ONE composition per site)

Stop assuming every site is `Nav · Hero · Services · About · Testimonial · Contact · Footer`. The right number and order of sections depends on the brief.

| ID | Composition | Sections (in order) | When |
|----|-------------|---------------------|------|
| **A** | **Manifesto + proof** | Nav · Hero · Single body of philosophy · Photo gallery · Long-form testimonial · Quiet contact line · Footer | Solo practitioners whose work is sold on philosophy + reputation. Few sections, lots of breathing room. |
| **B** | **Story arc** | Nav · Hero · Founding story · Journey timeline · Current work · Where they are today · Contact · Footer | Heritage businesses, multi-generational, businesses with a clear "from… to…" arc. |
| **C** | **Service-led** | Nav · Hero · Service grid · Why us · Testimonials · About · Contact · Footer | Multi-service businesses where the visitor's job is "find the right service". Cafes with menus, salons with treatments, mechanics with services. |
| **D** | **Transition** | Nav · Announcement hero · What's changing · What stays the same · Where to find us now · Contact · Footer | Businesses moving location, rebranding, launching, transitioning. |
| **E** | **Lineage** | Nav · Hero · Lineage / timeline · The work itself · Who teaches it now · How to learn / engage · Contact · Footer | Practitioners where lineage and teaching is the credibility. Hellerwork practitioners, traditional craft, master-apprentice trades. |
| **F** | **Portfolio-first** | Nav · Hero · Portfolio grid (most space) · Brief about · Contact · Footer | Photographers, designers, florists, makeup artists, anyone whose work is the pitch. |
| **G** | **Trust-and-quote** | Nav · Single-CTA hero · Trust strip with numbers · Single service description · Live quote / booking · Contact · Footer | Single-job trades: plumber call-out, lawn care, mobile mechanic, cleaner. The only thing that matters is "can I book this fast and trust them". |

**Rule:** the chosen composition determines the file structure. Don't add or drop sections at random. If a composition has 5 sections, the site has 5 sections.

---

## Motion grammar levels (pick ONE level per site)

Match the motion to the brief. A "calm" brand should not have a kinetic site, no matter how cool the animation is.

| ID | Level | Description | When |
|----|-------|-------------|------|
| **M-1** | **Stillness** | Zero or near-zero motion. Type and space do all the work. Maybe a single fade-up on first paint. | Practitioners whose brand is "quiet". Long-tenure solo practitioners. Heritage. Anything that needs to feel slow on purpose. |
| **M-2** | **Editorial** | Subtle: section fade-ups, slow image zooms on hover, soft parallax on hero photo, smooth nav scroll-state. | Mature, considered businesses. Most professional services. Healthcare, finance, established trades. Default for most sites. |
| **M-3** | **Kinetic** | Heavy: scroll-pinned sections, marquees, scrubbed timelines, magnetic interactions, type that animates on scroll. | Modern brands, youth-facing services, tech-adjacent trades. Sites where motion IS the brand. |
| **M-4** | **Cinematic** | Full-bleed video hero, scroll-driven canvas reveals, 3D-feel transitions. Reserved for the cinematic-sites pipeline normally. | High-end retail, premium auto, hospitality flagships. Use sparingly. |

**Rule:** never use M-3 or M-4 for HP-1 (text manifesto) or HP-4 (quote-first) — they fight each other.

---

## Photo identity-matching (HARD RULE — non-negotiable)

A site that pictures a different gender / age / ethnicity than the actual practitioner is not "almost right" — it is wrong, and it undermines the entire pitch the moment the lead notices.

**Rules:**

1. **If the business is identified with a specific named individual** (e.g. "Yasmin Lang", "Howard Rontal", "Dr Jane Reffell"), every photo that depicts a practitioner MUST either:
   - match that person's apparent gender, age range, and ethnicity, OR
   - be **anonymised** — close-crop on hands, back, technique only. No face, no full body, no shot that suggests practitioner identity.
2. **When in doubt, default to abstract/object photography** instead of generic stock people. Still life, anatomy diagrams, tools, materials, detail crops. This always beats a wrong-gender practitioner shot.
3. **Never use a photo that reads as a different gender to the named practitioner.** This is the most common failure mode and the most damaging.
4. Generic businesses (no named practitioner — e.g. "Quick Fix Automotive", "The Crossing Cafe") can use any appropriate stock photography.
5. Before committing to a photo, look at it. If you can't tell whether it matches the practitioner, swap to an anonymised crop or an object shot. A wrong photo is much worse than a missing one.

This rule applies to hero photos AND any internal/secondary photos, AND any decorative imagery.

---

## Variation enforcement (across the build log)

For any sector, the last 3 sites in the build log must NOT share more than 2 of these axes:
- hero pattern (HP-1 to HP-8)
- composition (A to G)
- motion grammar (M-1 to M-4)
- palette
- font pairing
- layout

If they do, re-roll. The whole point of this skill is "every site looks built for that specific business". Templates are the failure mode.

---

## Architecture Rules (NEVER BREAK)

- **One file only** — all CSS in `<style>`, all JS in `<script>`, no separate files
- **No frameworks** — no React, no Vue, no Tailwind build, no npm
- **CDN only** — Google Fonts, GSAP, Lucide Icons
- **No placeholder text** — write real copy from the scraped business data
- **6 sections always** — Navbar, Hero, Services, Philosophy, Contact, Footer
- **Dynamic copyright year** — never hardcode the year. Use `new Date().getFullYear()` in JS to set it

### Required CDN scripts
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://unpkg.com/lucide@latest"></script>
```

---

## DESIGN SYSTEM

Pick ONE from each column. Never repeat the same combo. Check `sites/build-log.md` first.

---

### COLOR PALETTES

**Dark:**

| Code | Name | Values |
|------|------|--------|
| P1 | Warm Night | `--bg:#080A0C --card:#111518 --accent:#C9A87C --text:#F2EDE8 --muted:#8A8B8D` |
| P2 | Deep Teal | `--bg:#0A1014 --card:#121A1F --accent:#5EADB5 --text:#E8F0F2 --muted:#7A9098` |
| P3 | Noir Plum | `--bg:#0C080E --card:#16111A --accent:#A8748E --text:#F0EBF0 --muted:#8A7E8C` |
| P4 | Slate Ember | `--bg:#0D0E10 --card:#161819 --accent:#C47A4A --text:#ECE8E4 --muted:#8B8985` |
| P5 | Midnight Forest | `--bg:#070B09 --card:#0F1612 --accent:#4CA879 --text:#E8F2EC --muted:#7A8E82` |

**Light:**

| Code | Name | Values |
|------|------|--------|
| P6 | Cream & Sage | `--bg:#F5F1EC --card:#FFFFFF --accent:#5B7A5E --text:#1A1A1A --muted:#6B6B6B` |
| P7 | Blush Editorial | `--bg:#FAFAF8 --card:#FFFFFF --accent:#BF8B8B --text:#2D2D2D --muted:#888888` |
| P8 | Warm Ivory | `--bg:#F8F4EF --card:#FFFDF9 --accent:#B8860B --text:#1C1914 --muted:#7A7468` |
| P9 | Cloud Lavender | `--bg:#F6F4F9 --card:#FFFFFF --accent:#7B68AE --text:#22202A --muted:#7E7A88` |
| P10 | Pearl Marine | `--bg:#F2F6F8 --card:#FFFFFF --accent:#3D7A8A --text:#1A2528 --muted:#6B7E85` |

---

### FONT PAIRINGS

| Code | Sans (Body) | Serif (Display) | Mood |
|------|-------------|-----------------|------|
| F1 | Outfit 300–700 | Cormorant Garamond italic | Elegant luxury |
| F2 | DM Sans 400–700 | Playfair Display italic | Classic editorial |
| F3 | Inter 300–700 | Lora italic | Clean modern |
| F4 | Sora 300–700 | Instrument Serif italic | Tech luxury |
| F5 | Plus Jakarta Sans 300–700 | Fraunces italic | Warm editorial |
| F6 | Manrope 300–700 | Bodoni Moda italic | High fashion |
| F7 | Space Grotesk 400–700 | Crimson Pro italic | Contemporary |
| F8 | Nunito Sans 300–700 | Libre Baskerville italic | Approachable classic |
| F9 | Figtree 300–700 | Noto Serif Display italic | Bold editorial |
| F10 | Rubik 300–700 | EB Garamond italic | Timeless warmth |

**Google Fonts URL pattern:**
```html
<link href="https://fonts.googleapis.com/css2?family=FONT_PARAMS&display=swap" rel="stylesheet">
```

---

### LAYOUTS

| Code | Hero | Cards | Navbar |
|------|------|-------|--------|
| L1 | Content bottom-left | 3-column grid | Pill-shaped floating |
| L2 | Centered text | 2-column zig-zag | Slim top-bar |
| L3 | Split-screen 50/50 | Horizontal divider rows | Full-width thin bar |
| L4 | Center-aligned stack | Full-width horizontal cards | Thin line top |
| L5 | Asymmetric 60/40 | Masonry 2-column | Thick bar, large logo |

---

### SECTOR-AWARE COMBO SELECTION (apply BEFORE picking from the tables above)

Don't randomly mix from all 10 palettes × 10 fonts × 5 layouts (500 combos). Filter first by sector, then pick a unique combo from the matching subset. This is the single biggest reason the old output looked generic — every business got the same lottery.

| Sector | Allowed palettes | Allowed fonts | Allowed layouts | Why |
|--------|------------------|---------------|-----------------|-----|
| Cafe / casual food / bakery | P6 P7 P8 P10 | F2 F5 F8 F10 | L1 L2 L4 | Warm, light, photo-led, friendly |
| Premium dining / fine dining | P1 P3 P4 | F1 F4 F6 F9 | L2 L3 L5 | Cinematic, dark, restrained |
| Hair / beauty / nails | P6 P7 P9 | F1 F2 F5 F6 | L1 L2 L4 | Soft, editorial, photo-grid |
| Day spa / massage / wellness | P6 P8 P10 | F3 F5 F8 F10 | L2 L4 | Calm, generous, light |
| Gym / fitness / pilates | P1 P2 P4 P5 | F4 F7 F9 | L3 L5 | Bold, dark, kinetic |
| Yoga / breathwork | P6 P8 P10 | F1 F3 F5 | L2 L4 | Calm, restrained, light |
| Plumber / electrician / mobile trade | P2 P4 P5 P10 | F3 F4 F7 F9 | L1 L4 | Clear, trust-first, single-CTA |
| Mechanic / panel / auto / tyre | P1 P2 P4 P5 | F4 F7 F9 | L3 L5 | Dark, premium, service-as-product |
| Builder / chippy / renovator | P1 P4 P5 | F4 F7 F9 | L3 L5 | Bold, dense, project-led |
| Florist / event styling | P6 P7 P9 | F1 F2 F5 | L1 L2 L4 | Soft, photo-grid, editorial |
| Photographer / videographer | P1 P3 P6 P7 | F1 F4 F6 | L2 L3 L4 | Portfolio-first, almost no chrome |
| Wedding venue / function space | P1 P3 P6 P7 | F1 F2 F6 | L2 L3 L5 | Cinematic, photo-led |
| Accommodation / B&B / cabins | P5 P6 P8 P10 | F2 F5 F8 F10 | L1 L2 L4 | Warm, listing-style |
| Tourism / experiences / charters | P2 P5 P10 | F3 F4 F7 | L3 L5 | Story-led, scene by scene |
| Family medical / GP / dental / allied health | P6 P8 P10 | F3 F8 F10 | L2 L4 | Soft, friendly, trust |
| Family restaurant / pub / club | P5 P6 P7 P8 | F5 F8 F10 | L1 L2 L4 | Warm, friendly, category-led |
| Childcare / kids' activities | P6 P7 P8 P9 | F5 F8 F10 | L1 L2 L4 | Warm, soft, photo-led |
| Specialist clinic / cosmetic / aesthetics | P3 P4 P6 P7 | F1 F4 F6 | L2 L4 L5 | Premium, restrained |
| Real estate agent | P1 P6 P7 P10 | F2 F4 F6 | L1 L3 L5 | Listing rhythm, premium |
| Retail boutique / homewares / fashion | P6 P7 P9 | F1 F2 F6 | L1 L2 L4 | Product cards, hero-led |
| Cleaning / lawn / garden / pool | P2 P5 P10 | F3 F7 F9 | L1 L4 | Clear, single-CTA, fast journey |
| Custom manufacturing / signage / specialist trade | P1 P4 P5 | F4 F7 F9 | L3 L5 | Dense, bold, project gallery |
| Heritage / multi-generational | P1 P3 P5 P8 | F2 F5 F10 | L2 L3 | Story-first, "since YEAR" framing |

If the sector isn't listed, pick the row that best matches by *feel* — a boutique gin distillery is closer to "premium dining" than to "manufacturing".

After narrowing by sector, then check `sites/build-log.md` for unique combos within that subset. If every combo in the subset is taken, expand to the next-closest row and reroll.

---

### PHOTOS (Unsplash — free, dynamic search)

Do NOT use a fixed photo bank. Instead, find photos relevant to each specific business.

**How to find photos:**
1. Determine the business type and category (e.g. nail salon, wedding venue, med spa)
2. Search Unsplash for relevant terms: `https://unsplash.com/s/photos/{search-term}`
3. Pick 2–3 photos that are relevant to what the business actually does
4. Use the direct image URL format: `https://images.unsplash.com/photo-{ID}?w={width}&h={height}&fit=crop&q=80`

**Rules:**
- Photos must be relevant to the business — a nail salon gets nail photos, a wedding venue gets wedding photos
- Use at least 2 photos per site, max 3
- Before using any photo, verify it loads by fetching `https://images.unsplash.com/photo-{ID}?w=400&q=60` and confirming a 200 status (not 404). Unsplash photos can be removed at any time.
- If a photo returns 404, find a replacement — never leave a broken image in the final site
- The hero photo should be high impact and relevant to the business atmosphere

---

## QUALITY STANDARDS (always applied)

- GSAP scroll-triggered fade-up animations on all sections
- Noise texture overlay (`opacity: 0.04` dark / `0.02` light)
- Magnetic button hover (`scale(1.03)`, `translateY(-1px)`)
- Responsive at 375px mobile and 1440px desktop
- Clickable `tel:` and `mailto:` links
- Navbar transitions on scroll

### JS Safety — prevent invisible cards
```css
/* Always include this */
.service-card { opacity: 1 !important; }
```
```js
// Always init Lucide before GSAP
try { lucide.createIcons(); } catch(e) {}
gsap.registerPlugin(ScrollTrigger);
```

---

## Output

- File: `sites/{business-slug}/index.html`
- Log entry appended to: `sites/build-log.md`

**Slug format:** lowercase, hyphenated. `Zen Nail Bar` → `zen-nail-bar`
