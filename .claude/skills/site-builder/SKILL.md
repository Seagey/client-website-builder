---
name: site-builder
description: Generate a single-file HTML/CSS/JS website from scratch for a new or existing business, driven by a DESIGN.md spec and a written brief. Uses a curated library of palettes, fonts, layouts, hero patterns, section compositions, and motion grammars to avoid AI-default look. Triggers on "site-builder", "build the site", "build the website", "generate the index.html".
---

# site-builder

Build one website. For one business. From the brand brief and DESIGN.md the orchestrator has already locked in. Single file: `index.html` with all CSS in `<style>` and all JS in `<script>`. No build step. No framework. Deployable to Vercel as-is.

This skill is the website generation engine. It is called by the `client-website` orchestrator after the brand book has been approved and the DESIGN.md has been written. It does not do intake. It does not do brand research. It assumes the inputs already exist.

---

## Inputs the skill expects

Before this skill runs, the following must exist in the project working directory:

1. **`DESIGN.md`** at the project root. Contains the locked colour palette, typography, spacing, rounded values, and component styles for the brand. Read this first.
2. **A brief.** Three sentences captured during intake:
   - What the business is actually selling (human story, not the category)
   - The single most distinctive differentiator
   - What the visitor needs to feel or learn in the first 10 seconds
3. **A business slug.** Lowercase, hyphenated. Used for the output folder name and the eventual Vercel project name.
4. **A logo or wordmark decision.** Either a file path to an SVG/PNG or a "wordmark only" instruction with the chosen font.
5. **Photography decision.** Either user-supplied photo paths or permission to use Unsplash via dynamic search.

If any of these are missing, stop. Tell the orchestrator what's missing. Do not invent.

---

## Process

### Step 1: Re-read the brief

The three-sentence brief drives every downstream choice. Read it again before opening any of the libraries below. If you cannot describe the business in your own words after reading the brief, ask for more context before continuing.

### Step 2: Identify the sector

The sector is a starting filter. It narrows which palettes, fonts, and layouts to consider. It does not determine the structural shape of the site. Two cafes can need radically different sites.

Use the sector table further down to narrow the candidate palettes, fonts, and layouts. If the sector is not listed, pick the closest row by feel.

### Step 3: Pick a hero pattern

Pick ONE hero pattern. The hero pattern is the structural shape of the first viewport. Do not default to "image right, text left." Pick the pattern that fits what this business actually is.

| ID | Pattern | When to use | Avoid when |
|----|---------|-------------|------------|
| HP-1 | Text manifesto. Large typographic statement, no hero image, single CTA, generous space. | Practitioners with a strong philosophy. Writers, consultants, designers, single-discipline therapists. When the words ARE the offer. | Visual-led businesses (florist, photographer). |
| HP-2 | Image-dominant. Full-bleed hero photo, small text overlay, restrained CTA. | Businesses where visuals ARE the offer: photographers, florists, interior designers, restaurants with strong food photography, accommodation. | When strong on-brand photography is not available. |
| HP-3 | Asymmetric split. 60/40 text-and-image, single product or service framing. | Service-as-product framing: a single hero offer (mechanic, premium dental, single-treatment clinic, signature cafe item). | Multi-service generalists. |
| HP-4 | Quote-first. Large pull-quote in serif takes the whole hero, photo and CTA delayed below the fold. | Businesses sold on testimonial or reputation. Long-tenure practitioners. Heritage businesses. | When the business has no real testimonials. |
| HP-5 | Announcement. The news IS the hero. Move, launch, transition, anniversary stated as the headline. | Businesses with a transition, launch, or single piece of news that is genuinely the most important thing for any visitor to know. | When there is no real news. |
| HP-6 | Timeline. Hero is a horizontal or vertical timeline of dates that scroll-scrubs. | Heritage or lineage businesses (40-year practitioner, multi-generational family business, "since 1972"). | Without real, specific dates. |
| HP-7 | Kinetic typography. Oversized type that scroll-animates, marquee, scrubbed letter reveals. | Modern, youth, tech-adjacent brands where motion IS the brand. Bold trades that lean future-facing. | Calm, quiet, mature brands. |
| HP-8 | Live data or state. Hero is the current schedule, the live availability, the latest gallery item, today's specials. | Businesses where the current state IS the value: bookings-heavy salons, restaurants with daily menus, gyms with class schedules. | Requires real fetchable data. Do not fake it. |

### Step 4: Pick a section composition

The composition determines the total number of sections and their order. Stop assuming every site is Nav, Hero, Services, About, Testimonial, Contact, Footer.

| ID | Composition | Sections (in order) | When |
|----|-------------|---------------------|------|
| A | Manifesto and proof | Nav, Hero, Single body of philosophy, Photo gallery, Long-form testimonial, Quiet contact line, Footer | Solo practitioners whose work is sold on philosophy and reputation. Few sections, lots of breathing room. |
| B | Story arc | Nav, Hero, Founding story, Journey timeline, Current work, Where they are today, Contact, Footer | Heritage businesses, multi-generational, businesses with a clear "from then to now" arc. |
| C | Service-led | Nav, Hero, Service grid, Why us, Testimonials, About, Contact, Footer | Multi-service businesses where the visitor's job is "find the right service". Cafes with menus, salons with treatments, mechanics with services. |
| D | Transition | Nav, Announcement hero, What's changing, What stays the same, Where to find us now, Contact, Footer | Businesses moving location, rebranding, launching, transitioning. |
| E | Lineage | Nav, Hero, Lineage or timeline, The work itself, Who teaches it now, How to learn or engage, Contact, Footer | Practitioners where lineage and teaching is the credibility. Traditional craft, master-apprentice trades. |
| F | Portfolio-first | Nav, Hero, Portfolio grid (most space), Brief about, Contact, Footer | Photographers, designers, florists, makeup artists, anyone whose work is the pitch. |
| G | Trust-and-quote | Nav, Single-CTA hero, Trust strip with numbers, Single service description, Live quote or booking, Contact, Footer | Single-job trades: plumber call-out, lawn care, mobile mechanic, cleaner. The only thing that matters is "can I book this fast and trust them". |

The chosen composition determines the file structure. Do not add or drop sections at random. If a composition has 5 sections, the site has 5 sections.

### Step 5: Pick a motion level

| ID | Level | Description | When |
|----|-------|-------------|------|
| M-1 | Stillness | Zero or near-zero motion. Type and space do all the work. Maybe a single fade-up on first paint. | Practitioners whose brand is "quiet". Long-tenure solo practitioners. Heritage. Anything that needs to feel slow on purpose. |
| M-2 | Editorial | Subtle: section fade-ups, slow image zooms on hover, soft parallax on hero photo, smooth nav scroll-state. | Mature, considered businesses. Most professional services. Healthcare, finance, established trades. Default for most sites. |
| M-3 | Kinetic | Heavy: scroll-pinned sections, marquees, scrubbed timelines, magnetic interactions, type that animates on scroll. | Modern brands, youth-facing services, tech-adjacent trades. Sites where motion IS the brand. |
| M-4 | Cinematic | Full-bleed video hero, scroll-driven canvas reveals, 3D-feel transitions. | High-end retail, premium auto, hospitality flagships. Use sparingly. |

Never use M-3 or M-4 with HP-1 (text manifesto) or HP-4 (quote-first). They fight each other.

### Step 6: Pick palette, fonts, layout from the sector subset

Read the DESIGN.md first. The brand's primary, secondary, and tertiary colours and the chosen font pairing are already locked. The site uses those exact values, NOT a re-pick from the libraries below.

The libraries below are reference material in case the orchestrator did not lock specific choices, or in case the DESIGN.md needs a starting point. If DESIGN.md is fully populated, skip to Step 7.

#### Palette library

Dark:

| Code | Name | Values |
|------|------|--------|
| P1 | Warm Night | `--bg:#080A0C --card:#111518 --accent:#C9A87C --text:#F2EDE8 --muted:#8A8B8D` |
| P2 | Deep Teal | `--bg:#0A1014 --card:#121A1F --accent:#5EADB5 --text:#E8F0F2 --muted:#7A9098` |
| P3 | Noir Plum | `--bg:#0C080E --card:#16111A --accent:#A8748E --text:#F0EBF0 --muted:#8A7E8C` |
| P4 | Slate Ember | `--bg:#0D0E10 --card:#161819 --accent:#C47A4A --text:#ECE8E4 --muted:#8B8985` |
| P5 | Midnight Forest | `--bg:#070B09 --card:#0F1612 --accent:#4CA879 --text:#E8F2EC --muted:#7A8E82` |

Light:

| Code | Name | Values |
|------|------|--------|
| P6 | Cream and Sage | `--bg:#F5F1EC --card:#FFFFFF --accent:#5B7A5E --text:#1A1A1A --muted:#6B6B6B` |
| P7 | Blush Editorial | `--bg:#FAFAF8 --card:#FFFFFF --accent:#BF8B8B --text:#2D2D2D --muted:#888888` |
| P8 | Warm Ivory | `--bg:#F8F4EF --card:#FFFDF9 --accent:#B8860B --text:#1C1914 --muted:#7A7468` |
| P9 | Cloud Lavender | `--bg:#F6F4F9 --card:#FFFFFF --accent:#7B68AE --text:#22202A --muted:#7E7A88` |
| P10 | Pearl Marine | `--bg:#F2F6F8 --card:#FFFFFF --accent:#3D7A8A --text:#1A2528 --muted:#6B7E85` |

#### Font pairings

| Code | Sans (body) | Serif (display) | Mood |
|------|-------------|-----------------|------|
| F1 | Outfit 300 to 700 | Cormorant Garamond italic | Elegant luxury |
| F2 | DM Sans 400 to 700 | Playfair Display italic | Classic editorial |
| F3 | Inter 300 to 700 | Lora italic | Clean modern |
| F4 | Sora 300 to 700 | Instrument Serif italic | Tech luxury |
| F5 | Plus Jakarta Sans 300 to 700 | Fraunces italic | Warm editorial |
| F6 | Manrope 300 to 700 | Bodoni Moda italic | High fashion |
| F7 | Space Grotesk 400 to 700 | Crimson Pro italic | Contemporary |
| F8 | Nunito Sans 300 to 700 | Libre Baskerville italic | Approachable classic |
| F9 | Figtree 300 to 700 | Noto Serif Display italic | Bold editorial |
| F10 | Rubik 300 to 700 | EB Garamond italic | Timeless warmth |

Google Fonts URL pattern:

```html
<link href="https://fonts.googleapis.com/css2?family=FONT_PARAMS&display=swap" rel="stylesheet">
```

Never use Inter alone. Never use Roboto. Never use system-ui as the primary face. Always pair a body face with a display face.

#### Layout library

| Code | Hero | Cards | Navbar |
|------|------|-------|--------|
| L1 | Content bottom-left | 3-column grid | Pill-shaped floating |
| L2 | Centered text | 2-column zig-zag | Slim top-bar |
| L3 | Split-screen 50/50 | Horizontal divider rows | Full-width thin bar |
| L4 | Center-aligned stack | Full-width horizontal cards | Thin line top |
| L5 | Asymmetric 60/40 | Masonry 2-column | Thick bar, large logo |

#### Sector subset map

| Sector | Allowed palettes | Allowed fonts | Allowed layouts | Why |
|--------|------------------|---------------|-----------------|-----|
| Cafe, casual food, bakery | P6 P7 P8 P10 | F2 F5 F8 F10 | L1 L2 L4 | Warm, light, photo-led, friendly |
| Premium dining, fine dining | P1 P3 P4 | F1 F4 F6 F9 | L2 L3 L5 | Cinematic, dark, restrained |
| Hair, beauty, nails | P6 P7 P9 | F1 F2 F5 F6 | L1 L2 L4 | Soft, editorial, photo-grid |
| Day spa, massage, wellness | P6 P8 P10 | F3 F5 F8 F10 | L2 L4 | Calm, generous, light |
| Gym, fitness, pilates | P1 P2 P4 P5 | F4 F7 F9 | L3 L5 | Bold, dark, kinetic |
| Yoga, breathwork | P6 P8 P10 | F1 F3 F5 | L2 L4 | Calm, restrained, light |
| Plumber, electrician, mobile trade | P2 P4 P5 P10 | F3 F4 F7 F9 | L1 L4 | Clear, trust-first, single-CTA |
| Mechanic, panel, auto, tyre | P1 P2 P4 P5 | F4 F7 F9 | L3 L5 | Dark, premium, service-as-product |
| Builder, chippy, renovator | P1 P4 P5 | F4 F7 F9 | L3 L5 | Bold, dense, project-led |
| Florist, event styling | P6 P7 P9 | F1 F2 F5 | L1 L2 L4 | Soft, photo-grid, editorial |
| Photographer, videographer | P1 P3 P6 P7 | F1 F4 F6 | L2 L3 L4 | Portfolio-first, almost no chrome |
| Wedding venue, function space | P1 P3 P6 P7 | F1 F2 F6 | L2 L3 L5 | Cinematic, photo-led |
| Accommodation, B&B, cabins | P5 P6 P8 P10 | F2 F5 F8 F10 | L1 L2 L4 | Warm, listing-style |
| Tourism, experiences, charters | P2 P5 P10 | F3 F4 F7 | L3 L5 | Story-led, scene by scene |
| Family medical, GP, dental, allied health | P6 P8 P10 | F3 F8 F10 | L2 L4 | Soft, friendly, trust |
| Family restaurant, pub, club | P5 P6 P7 P8 | F5 F8 F10 | L1 L2 L4 | Warm, friendly, category-led |
| Childcare, kids' activities | P6 P7 P8 P9 | F5 F8 F10 | L1 L2 L4 | Warm, soft, photo-led |
| Specialist clinic, cosmetic, aesthetics | P3 P4 P6 P7 | F1 F4 F6 | L2 L4 L5 | Premium, restrained |
| Real estate agent | P1 P6 P7 P10 | F2 F4 F6 | L1 L3 L5 | Listing rhythm, premium |
| Retail boutique, homewares, fashion | P6 P7 P9 | F1 F2 F6 | L1 L2 L4 | Product cards, hero-led |
| Cleaning, lawn, garden, pool | P2 P5 P10 | F3 F7 F9 | L1 L4 | Clear, single-CTA, fast journey |
| Custom manufacturing, signage, specialist trade | P1 P4 P5 | F4 F7 F9 | L3 L5 | Dense, bold, project gallery |
| Heritage, multi-generational | P1 P3 P5 P8 | F2 F5 F10 | L2 L3 | Story-first, "since YEAR" framing |

If the sector is not listed, pick the closest row by feel.

### Step 7: Photography

Photography rules. Read these before you pick any image.

If the business is identified with a specific named individual, every photo that depicts a practitioner must either:
- match that person's apparent gender, age range, and ethnicity, OR
- be anonymised, close-crop on hands, back, technique only. No face, no full body, no shot that suggests practitioner identity.

When in doubt, default to abstract or object photography rather than generic stock people. Still life, anatomy diagrams, tools, materials, detail crops. This always beats a wrong-gender practitioner shot.

Never use a photo that reads as a different gender to the named practitioner. This is the most common failure mode and the most damaging.

Generic businesses (no named practitioner) can use any appropriate stock photography.

Before committing to a photo, look at it. If you cannot tell whether it matches the practitioner, swap to an anonymised crop or an object shot. A wrong photo is much worse than a missing one.

#### Sourcing photos

If the user has supplied their own photography, use that.

If not, use Unsplash via dynamic search:

1. Determine the business type and the most important visual subjects (e.g. nail salon, wedding venue, day spa)
2. Search Unsplash: `https://unsplash.com/s/photos/{search-term}`
3. Pick 2 to 3 photos that are relevant to what the business actually does
4. Use the direct image URL: `https://images.unsplash.com/photo-{ID}?w={width}&h={height}&fit=crop&q=80`
5. Before using any photo, verify it loads (fetch the URL, expect a 200 status). Unsplash photos can be removed at any time.
6. If a photo returns 404, find a replacement. Never leave a broken image in the final site.

Use at least 2 photos. Maximum 3. More than 3 starts to feel templated.

### Step 8: Generate the file

Output: `sites/{business-slug}/index.html`

Architecture rules. Non-negotiable.

- One file only. All CSS in `<style>`, all JS in `<script>`, no separate files.
- No frameworks. No React, no Vue, no Tailwind build, no npm.
- CDN only for fonts and libraries. Google Fonts, GSAP, Lucide Icons.
- No placeholder text. Every line of copy is real, written for this business, in their voice.
- Dynamic copyright year. Never hardcode. Use `new Date().getFullYear()` in JS to set it.
- Tokens come from DESIGN.md. Re-use the colours and fonts locked there. Do not invent new hex codes mid-build.
- AU English in all copy. Colour, organise, centre, behaviour, optimise, realise.
- No em dashes (—) or en dashes (–) anywhere in copy or comments. Use commas, full stops, parentheses, or "and" / "but".
- Mobile and desktop both look intentional. Mentally test at 375px and 1440px. Do not ship a desktop-only design with a broken mobile fallback.
- One primary CTA. Not 14 different buttons.

Required CDN scripts:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://unpkg.com/lucide@latest"></script>
```

Quality standards applied to every site:

- GSAP scroll-triggered fade-ups on all sections
- Noise texture overlay (`opacity: 0.04` on dark themes, `0.02` on light themes)
- Magnetic button hover (`scale(1.03)`, `translateY(-1px)`)
- Responsive at 375px mobile and 1440px desktop
- Clickable `tel:` and `mailto:` links
- Navbar transitions on scroll

JS safety:

```css
/* Always include this to prevent invisible cards */
.service-card { opacity: 1 !important; }
```

```js
// Always init Lucide before GSAP
try { lucide.createIcons(); } catch(e) {}
gsap.registerPlugin(ScrollTrigger);
```

### Step 9: Preview locally

Before handing the site to the deploy step, open the file in a browser and scroll through it. Check:

- Hero loads correctly. Photo not broken.
- All sections visible. Nothing stuck at `opacity: 0`.
- Mobile layout works at 375px width.
- Copy is real. No placeholder text snuck in.
- Animations fire on scroll but don't block readability.
- All links work (tel:, mailto:, anchor links to sections).

If anything looks wrong, fix before deploying.

---

## Structural inspiration

For layout, hierarchy, spacing rhythm, hero treatment, card styles, and scroll feel, consult the `site-design-references` skill in this kit. Hard rules:

- Structural inspiration only. Never lift colours, fonts, copy, or imagery.
- Never name a reference site in the output (no comments saying "inspired by Apple").
- Use the sector map in `site-design-references` to pick appropriate references.

---

## Output

- File: `sites/{business-slug}/index.html`

Slug format: lowercase, hyphenated. `Sharni Co` → `sharni-co`. `The Northern Rivers Wellness Studio` → `northern-rivers-wellness-studio`.

After the file is written, the orchestrator typically previews it locally with the user and offers `tweak-html` for adjustments before passing the slug to `vercel-deploy`.
