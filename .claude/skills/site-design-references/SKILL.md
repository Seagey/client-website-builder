---
name: site-design-references
description: Curated, filtered set of layout and structural references for premium local-business websites. Use as silent inspiration when picking layout/hierarchy/spacing for a site-redesign. Triggers on "design references", "site references", "layout inspiration".
trigger: "site-design-references" or "design references" or "site references" or "layout inspiration"
---

# Skill: Site Design References

A small, opinionated reference library of premium consumer-facing websites worth studying when redesigning a site for a local business (cafe, plumber, gym, beautician, mechanic, hospitality, retail). Lives alongside the much larger upstream library at github.com/VoltAgent/awesome-design-md → getdesign.md, but filtered heavily so we only inherit patterns that translate to local-business sites, never SaaS, dashboards, or developer tools.

---

## HARD RULES, read these every time

1. **Structural inspiration only.** Use these references to inform LAYOUT, HIERARCHY, SPACING RHYTHM, HERO TREATMENT, CARD STYLES, SCROLL FEEL.
2. **Never lift visuals.** Do not copy a brand's exact colours, fonts, logo, or imagery. Each redesign must feel like the LOCAL BUSINESS we're building for, not like Apple-with-a-different-logo.
3. **Never lift copy.** No taglines, no slogans, no headline phrasing from a reference brand. All copy comes from the brief and notes captured during intake.
4. **Never name the reference in the output.** The redesign must not reference the inspiration site by name in the HTML, the comments, or anywhere visible to the client.
5. **No SaaS, no dashboards, no dev tools.** This library has been pre-filtered to exclude them. Don't reach for the upstream library and pull something like Linear, Stripe, Vercel, Notion, Figma, those are dashboard layouts and they make local-business sites feel cold, busy, and wrong.
6. **Only use what fits the sector.** Use the sector map below, don't apply BMW patterns to a yoga studio.

---

## Whitelist, the only references to consult

These are pre-vetted as appropriate for premium local-business sites. The full upstream library remains browsable at github.com/VoltAgent/awesome-design-md if a future entry needs vetting in.

### Hospitality / lifestyle

- **airbnb**, listing cards, photography-led grids, soft rounded corners, search-first hero, generous photo ratios. Lessons: how to make photos lead the hero; how to present a list of "things you can do here" without it feeling like a menu. *Apply to:* hospitality, accommodation, venues, tourism, day spas, larger restaurants, wedding venues.
- **pinterest**, masonry visual grid, no-chrome cards, content-first layout. Lessons: how to let imagery do all the work without typography fighting it; staggered grid rhythm. *Apply to:* portfolio-driven sectors (photographers, florists, hairdressers, tattoo studios, makeup artists, bakers, interior designers).

### Premium product / craft

- **apple**, full-bleed hero with one product, generous negative space, tight type pairing, large numbers/specs as feature highlights, sticky-on-scroll product reveals. Lessons: confidence through restraint; let one image dominate. *Apply to:* anything where the offer is a single hero product or service the business takes pride in (boutique cafes with a flagship coffee, gyms with a signature class, spas with a signature treatment).
- **bmw**, premium product page rhythm, dark moody hero, scroll-driven story (specs unfold), strong typography hierarchy. Lessons: a service can be presented like a product. *Apply to:* mechanics, auto detailing, panel beaters, motorcycle, marine, anything mechanical and proud.
- **ferrari**, cinematic hero photography, deep blacks, italic serif accents over sans body, racing-stripe geometry, drama. Lessons: how to make a small business feel like a flagship. *Apply to:* high-end dining, premium auto, luxury fitness, anything where "exclusive" is part of the pitch.
- **lamborghini**, sharp angular sections, asymmetric splits, kinetic feel even in still layouts. Lessons: angular section dividers and split layouts can give a static site movement. *Apply to:* sectors that want to feel bold and modern (signage, custom builders, performance gyms).
- **renault**, friendlier consumer-auto patterns, lighter palette range, simpler navigation. Lessons: a softer take on the same "service-as-product" frame. *Apply to:* family-oriented service businesses (childcare-adjacent, family restaurants, kid's activities, GP-style health).
- **tesla**, minimal hero with one massive image, ALL-CAPS micro-labels, single-CTA hero. Lessons: ruthless simplification of the hero, one image, one heading, one button. *Apply to:* sectors that benefit from a "one thing only" message (single-service trades, specialist clinics, single-product cafes).

### Aspirational / story-led

- **spacex**, cinematic full-screen sections, large rich blacks, sparing accent colour, hero copy that reads like a manifesto. Lessons: tell a story scene by scene; let each scroll be a moment. *Apply to:* sectors with a strong founder story (family businesses with heritage, anything with "since 1972" credibility).
- **spotify**, content-card grids, bold blocks of colour as section backgrounds, oversized headings. Lessons: a page can read like a playlist of categories. *Apply to:* sectors with multiple service tiers/categories (hair salons with multiple treatments, fitness studios with multiple class types, restaurants with multiple cuisines).

### Service marketplaces / journey

- **uber**, clean service-first hero, a single primary action (book/get a quote), trust strip with numbers. Lessons: how to put one job-to-be-done above all else. *Apply to:* trade services where the primary action is "get a quote" or "book a job" (plumbing, electrical, cleaning, lawn care, mobile mechanics).

### Premium consumer fintech (only for "trust" patterns)

- **revolut**, cards with subtle gradients, trust-badge strips, oversized numbers as social proof. Lessons: how to present numbers (jobs done, customers served, years in business) at scale. *Apply to:* any business with strong numeric proof (e.g. "20 years local", "500 weddings catered"). Only borrow the SOCIAL-PROOF and TRUST patterns, never the fintech aesthetic.
- **wise**, straightforward conversion-first landing rhythm, friendly accent colour, clear CTAs. Lessons: how to keep a 6-section page feeling brisk. *Apply to:* same as revolut.

### Tooling / craft / dev-adjacent (use sparingly, structural only)

- **warp**, terminal-meets-marketing landing, dense feature grid, dark mode hero with bright accent. Lessons: how a dense feature list can still breathe. *Apply to:* SPECIALIST trades only (custom build, niche manufacturing, specialist repair) where dense detail is a feature, not a bug. Skip for hospitality, beauty, food.

---

## Sector → reference map

Use this when picking which references to consult for a given lead.

| Sector | Primary references | Vibe |
|--------|-------------------|------|
| Cafe / casual food | airbnb, spotify | Warm, photo-led, a category for each food type |
| Premium dining / fine dining | ferrari, apple | Cinematic, sparse, one-image-dominant hero |
| Bakery / patisserie / specialty food | pinterest, airbnb | Visual grid of products, soft + warm |
| Hair / beauty / nails | pinterest, spotify | Service-tier grid, photo-led, soft |
| Day spa / massage / wellness | airbnb, apple | Calm, generous space, hero photography |
| Gym / fitness / pilates | tesla, spotify | Bold hero, class grid, strong type |
| Yoga / breathwork / studio practice | apple, airbnb | Calm restraint, generous space |
| Plumber / electrician / sparky / mobile trade | uber, tesla | Single CTA, "get a quote" first, clear pricing |
| Mechanic / panel / auto detail / tyre | bmw, tesla | Service-as-product, dark + premium |
| Builder / chippy / renovator | lamborghini, warp | Bold sections, project gallery, dense detail |
| Florist / event styling | pinterest, airbnb | Visual grid, soft palette |
| Photographer / videographer | pinterest, apple | Portfolio first, almost no chrome |
| Wedding venue / function space | airbnb, ferrari | Cinematic hero, photo-led, "imagine your day" |
| Accommodation / B&B / cabins | airbnb | Listing patterns, photo-led |
| Tourism / experiences / charters | airbnb, spacex | Story-driven scrolls, scene by scene |
| Family medical / GP / dental / allied health | renault, apple | Soft, friendly, generous space, trust elements |
| Family restaurant / pub / club | renault, spotify | Friendly, category-led, lots of photos |
| Childcare / kids' activities | renault, pinterest | Warm, friendly, photo-led, soft |
| Specialist clinic (cosmetic, aesthetics, fertility) | apple, ferrari | Premium, restrained, hero-first |
| Real estate agent | airbnb, apple | Listing rhythm + premium feel |
| Retail boutique / homewares / fashion | apple, pinterest | Product cards, hero-led, clean |
| Cleaning / lawn / garden / pool care | uber, wise | Single CTA, trust badges, fast journey |
| Custom manufacturing / signage / specialist trade | warp, lamborghini | Dense + bold, project gallery |
| Heritage / multi-generational businesses | spacex, ferrari | Story, "since YEAR" framing, cinematic |

If the sector isn't in the table, pick the row that's closest in *feel*, not the one closest in industry. A boutique gin distillery is closer to "premium dining" than to "manufacturing".

---

## How to use in site-builder

1. **Determine the sector** from the brief and the business description captured during intake.
2. **Look up 1-2 primary references** in the sector map.
3. **Open the relevant brand notes above** and pick 2-3 specific structural patterns to inherit (e.g. "airbnb's photo-led hero" plus "pinterest's masonry grid").
4. **Translate to the user's business.** Apply those patterns to their actual content, photos, and brand. Use the colours and fonts locked in DESIGN.md. Do not use the reference brand's colours, fonts, or copy.
5. **Never name the reference in the output.** The brief was "study X, build for Y", and only Y appears in the deliverable.

---

## What NOT to import from upstream

The upstream getdesign.md library has 59 entries. The following are explicitly excluded from this skill, do not consult them:

**SaaS / dashboards / dev tools (excluded, wrong aesthetic for local biz):**
airtable, cal, claude, clay, clickhouse, cohere, composio, cursor, elevenlabs, expo, figma, framer, hashicorp, ibm, intercom, kraken, linear.app, lovable, minimax, mintlify, miro, mistral.ai, mongodb, notion, nvidia, ollama, opencode.ai, pinterest (used selectively, see whitelist), posthog, raycast, replicate, resend, runwayml, sanity, semrush, sentry, stripe, supabase, superhuman, together.ai, voltagent, webflow, x.ai, zapier

**Crypto / coinbase / financial-trader aesthetics (excluded, too cold for local biz):**
coinbase

**Acceptable but not whitelisted by default:**
Anything from the upstream library can be added later by extending this skill. Default is "don't consult".
