---
name: design-system
description: Generate a full design system reference page AND a 1-page A4 brand book PDF for a brand. Works from either an existing reference (site URL, screenshot, brand assets) OR from locked intake notes captured during client-website Phase 1b (chosen palette, fonts, business name, tagline). Triggers on "design system", "brand book", "make design assets", "extract design", "brand page", "one page brand".
---

# Design System and Brand Book Generator

Produces two artifacts from a brand source:

1. **`design-system.html`** at `design/design-system.html`. A scrollable reference page covering colours, typography, principles, components, icons, and wordmarks.
2. **`brand-book-a4.html`** rendered to **`brand-book-a4.pdf`** at `design/brand-book-a4.pdf`. A single A4 portrait page fitting the brand topline onto one shareable poster, print-ready.

Both outputs are self-contained HTML (Google Fonts CDN plus inline CSS plus inline SVG, no build step). The PDF is rendered via headless Edge or Chrome.

## Two valid sources

This skill works from either of two source types. Both are valid. Both are "the reference".

**Source A: existing reference.** The user provides a site URL, a screenshot, or existing brand assets. The skill extracts colours, fonts, tagline, logo, etc. This is the original use case.

**Source B: intake notes.** The user (via the `client-website` orchestrator) has already gone through Phase 1 intake and Phase 1a brand research, and Phase 1b has locked the values: business name, tagline, primary/secondary/tertiary hex codes, font pairing, logo decision. These locked values are passed in as the source. The skill renders them into the brand book and design system page.

In both cases, **nothing in this skill is invented**. Source A extracts from what already exists. Source B renders what the user has already chosen.

If neither source is provided, stop and ask which one the user wants to give. Do not invent a brand to fill a blank slate.

---

## Workflow

### Path A: working from an existing reference

1. **Get the reference.** URL, screenshot, existing site, or a description of the brand.
2. **Extract.** If URL, WebFetch it and inspect the HTML and CSS. If screenshot, Read it and identify colours and fonts visually. Pull out:
   - Hex colours (primary, secondary, any accents the brand actually uses)
   - Fonts (check `<link>` tags, CSS font-family declarations, or visual identification)
   - Tagline or value prop
   - Any documented principles
   - Logo or mark (grab as inline SVG if the reference has one)
   - Theme direction (light or dark, which surfaces are used)
3. **Confirm extraction.** Short message listing what you found. Ask only for the gaps.
4. **Generate both files** into a `design/` folder.
5. **Render the PDF** via headless Edge or Chrome.
6. **Show the user.** Send the PDF and the design-system.html.
7. **Iterate** on feedback.

### Path B: working from intake notes (the client-website Phase 1b case)

If the caller is the `client-website` orchestrator and Phase 1b has locked values, this skill receives the locked brand directly. There is no extraction step. The skill renders the locked values into the two artifacts.

1. **Read the locked values.** From the orchestrator's intake summary: business name, tagline, primary hex, secondary hex, tertiary hex, font pairing (display and body), logo path or "wordmark only" decision, the 10-second brief.
2. **Generate both files** into the `design/` folder, using the locked values as-is. Do not invent additional colours, fonts, principles, or taglines beyond what was locked.
3. **Render the PDF** via headless Edge or Chrome.
4. **Show the user.** Send the PDF and the design-system.html.
5. **Iterate** on feedback. If the user wants to change a colour or font, update the locked values via the orchestrator, then re-render.

In Path B, the `Principles` section can be skipped if no principles were captured during intake. Better to skip than to fill with generic invention.

---

## What to ask for (only if not extractable, only in Path A)

- **Brand name(s).** Single brand or a family (parent, product, studio).
- **Tagline.** One short line.
- **Principles.** 4 to 6 design rules. If the brand doesn't publish any, skip the section rather than invent.
- **Team or product set** (optional). 4 to 8 items with a colour and role each.
- **Any colours or fonts you can't see** in the reference.

In Path B (intake notes from `client-website`), the orchestrator has already collected these. Do not re-ask.

**Never invent brand details.** Colours, fonts, names, taglines, logos, and principles all come from the user, the reference, or the locked intake. If a section has no source material, drop it. Do not fill with made-up content.

**Never redraw logos.** Use the exact SVG from the reference, or the logo file the user supplied during intake. If the logo is a raster image, ask the user for the SVG or leave a placeholder and note it.

---

## Output 1: `design-system.html` (scrollable reference page)

Sections in this order (drop any for which there's no source material):

1. **Header**, brand lockup + tagline + version/date
2. **Color Hierarchy**, the primary, secondary, tertiary colors the brand actually uses, with hex + usage note
3. **Extended Palette**, any additional brand colors (status, category, etc)
4. **Typography**, display, body, mono samples using the brand's actual fonts; weight ladder
5. **Principles**, whatever the brand documents
6. **Components**, buttons, cards, badges styled in the brand's language
7. **Icons**, whatever icon language the brand uses (inline SVG, pixel art, line, filled, etc)
8. **Wordmarks / Lockups**, 2 to 3 name treatments based on the brand's mark + wordmark
9. **Footer**, file path, version, any cross-reference note

Match the brand's surfaces (light vs dark), radius, border treatment, and type hierarchy. If the brand is rounded and soft, don't default to sharp. If the brand is maximalist, don't make it minimal.

---

## Output 2: `brand-book-a4.html` → `brand-book-a4.pdf`

A **single A4 portrait page** (210mm × 297mm), the brand topline in one glance.

### Required print CSS

```css
@page { size: A4; margin: 0; }
* { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
.page { width: 210mm; height: 297mm; padding: 18mm 16mm 14mm; }
```

### Layout (top to bottom, single A4 page)

1. **Head**, brand lockup on the left, version + tagline on the right
2. **Color Hierarchy**, 3 swatch tiles (Primary / Secondary / Tertiary) using the brand's actual colors
3. **Two-column middle**, Typography card (brand's fonts) + Principles card (brand's documented principles)
4. **Team / Product row** (optional), 6 small tiles if the brand has a product family
5. **Wordmarks · Lockups**, 3 tiles showing name treatments
6. **Footer**, file path + cross-reference note

### Rendering to PDF

Windows (Edge):
```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brand-book-a4.pdf" \
  "file:///absolute/path/to/brand-book-a4.html"
```

macOS (Chrome):
```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brand-book-a4.pdf" \
  "file://$PWD/brand-book-a4.html"
```

Linux (Chromium):
```bash
chromium --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brand-book-a4.pdf" \
  "file://$PWD/brand-book-a4.html"
```

---

## Wordmark Patterns

Three lockup styles you can offer (adapt to the brand's tone):

1. **Mark + wordmark**, icon next to the brand name. Weight/tracking matches the brand's own wordmark.
2. **Split-weight lockup**, `PART1PART2` where one half is heavy and the other half is light. Useful for umbrella/group names.
3. **Accent lockup**, `PART1PART2` where one half is colored with the brand primary. Useful for studios/sub-brands.

Only include lockup variants that make sense for the brand. A single-word brand doesn't need a split-weight treatment.

---

## Skeleton Template

See `examples/template.html` for a structural skeleton with placeholder tokens (`{{PRIMARY}}`, `{{DISPLAY_FONT}}`, etc). Copy it, swap the tokens with values from your extraction, add or drop sections to match the brand.

---

## Rules

- **House style for all generated copy.** Australian English spelling (colour, organise, centre, behaviour, etc). No em dashes or en dashes anywhere in output. Use commas, full stops, or parentheses instead. Applies to taglines, principles, section labels, and any prose the skill writes. Does not apply to brand details extracted verbatim from the reference.
- **Extract before asking.** Use the reference to pre-fill whatever you can.
- **Never invent brand details.** No made-up colors, fonts, names, taglines, principles, or logos.
- **Never redraw logos.** Use the exact SVG the user provides.
- **Drop sections you can't source.** Better to skip a section than fill it with fiction.
- **Match the brand, don't impose a style.** The template is structural, visual language comes from the reference.
- **Fit on one A4 page.** If content overflows, tighten padding or drop a section.
- **Self-contained HTML.** Google Fonts CDN + inline CSS + inline SVG. No build step.
- **Render the PDF.** Don't just hand over HTML, finish the job with headless Edge/Chrome.
