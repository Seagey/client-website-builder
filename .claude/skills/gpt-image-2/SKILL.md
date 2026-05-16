---
name: gpt-image-2
description: Generate images (logos, signage, mockups, brand imagery) using OpenAI's GPT Image 2 model. Default provider is Fal AI (`FAL_KEY`). If the user does not have a Fal key, the skill falls back to Kie.ai (`KIE_AI_KEY`) which exposes the same model. Best-in-class for legible text inside images (chalkboards, signs, menus, posters, packaging, UI mockups). Triggers on "gpt-image-2", "generate logo", "make a logo", "create a poster", "design a sign", "make a brand image".
---

# gpt-image-2

OpenAI's image model, ideal for legible text inside images. In this kit it is primarily used to generate logo concepts and brand imagery during Phase 1a of `client-website` when the user does not already have a logo.

The skill works via one of two providers. It checks for an API key in this order:

1. **Fal AI** (`FAL_KEY` env var). Preferred. Sync API, fast, well-documented.
2. **Kie.ai** (`KIE_AI_KEY` env var). Fallback. Same model, different gateway.

If neither key is present, the skill stops and prompts the user to set one up. It does not silently fail and it does not invent images locally.

---

## First-run setup check

Before generating anything, run this check:

```bash
if [ -n "$FAL_KEY" ]; then
  echo "Using Fal AI"
elif [ -n "$KIE_AI_KEY" ]; then
  echo "Using Kie.ai"
else
  echo "No image generation key found."
fi
```

If neither key is set, tell the user something like:

> "Image generation needs an API key. You have two options:
>
> 1. **Fal AI** (recommended). Sign up at https://fal.ai, grab a key from your dashboard, and add it to your shell as `FAL_KEY` or to a project `.env` file.
> 2. **Kie.ai** (alternative). Sign up at https://kie.ai, get a key, and set it as `KIE_AI_KEY`.
>
> Both expose the same OpenAI GPT Image 2 model. Pick whichever you already have or whichever is easier to set up. Costs are roughly equivalent: ~5 cents USD per image at medium quality."

Wait for the user to confirm a key is set before continuing.

---

## How to load the key

Load from environment, in this order. Do not hardcode any path. Do not assume a particular `.env` location.

```bash
# Bash
KEY="${FAL_KEY:-$KIE_AI_KEY}"
if [ -z "$KEY" ]; then
  echo "No key found. Set FAL_KEY or KIE_AI_KEY."
  exit 1
fi
```

```python
# Python
import os
key = os.environ.get("FAL_KEY") or os.environ.get("KIE_AI_KEY")
if not key:
    raise RuntimeError("No key found. Set FAL_KEY or KIE_AI_KEY.")
```

```javascript
// Node
const key = process.env.FAL_KEY || process.env.KIE_AI_KEY;
if (!key) {
  throw new Error("No key found. Set FAL_KEY or KIE_AI_KEY.");
}
```

If the project has a `.env` file at its root, load it with `dotenv` or equivalent before reading the env vars. Never read a key from outside the project directory.

---

## Cost discipline

- **Medium quality** is the default. ~5 cents USD per image.
- **High quality** only when asked. ~15 cents per image.
- **Always state the cost** before generating. Example: "I'm about to generate 3 logo concepts at medium quality (~15 cents total). OK to proceed?"
- Wait for explicit approval before any generation call.

---

## Logo generation (the most common use in this kit)

When generating logo concepts for a client website project:

1. Read the brand brief (the three-sentence brief from `client-website` Phase 1).
2. Read the chosen colour direction. If colours are not locked yet, ask the user for a rough direction (warm, cool, earthy, bold, muted).
3. Write a prompt that describes the BUSINESS and the FEEL, not just "a logo". A weak prompt produces a weak logo.
4. Generate 2 to 3 concepts. Vary one axis between them (e.g. mark-led vs wordmark-led, geometric vs organic).
5. Save each to disk. Logo files belong in the project's `design/logos/` folder.
6. Present them to the user. Let them pick one. Iterate if needed.

### Prompt structure for logos

A good prompt has these parts:

- **What the business is.** Not just the category. "A 30-year solo Hellerwork practitioner who works one-on-one in a quiet studio", not "wellness clinic".
- **The feel.** One or two adjectives that come from the brief. "Quiet, considered, mature." Not "modern and clean and innovative", which produces AI slop.
- **Style direction.** "Simple wordmark in a serif", "Geometric mark with the initial 'S'", "Hand-drawn line illustration of a single leaf".
- **Colour.** Use the locked brand colours if available, otherwise describe in plain language: "deep forest green on cream".
- **What to avoid.** "No gradient mesh, no 3D bevel, no stock-photo lighting, no generic startup geometric mark."

### Example logo prompt

> "Simple wordmark logo for a solo yoga and breathwork practitioner. The practice is quiet, slow, one-on-one, focused on long-term clients. The wordmark reads 'Anya Wells' in a contemporary serif italic. Below the wordmark, a small graphic mark: a single horizontal line that suggests the horizon at dusk. Colour: muted sage green (#5B7A5E) on cream background (#F5F1EC). The whole composition should feel still, unhurried, considered. Avoid bevels, drop shadows, gradient mesh, generic geometric marks."

---

## Output locations

Logos and brand imagery for the current project go in:

```
design/logos/{label}_{ts}.png
design/imagery/{label}_{ts}.png
```

`{label}` is a short slug describing what the image is (`wordmark`, `mark-v1`, `hero-photo`). `{ts}` is a timestamp.

Do not save outside the current project directory. Each project owns its own design assets.

---

## API endpoints

### Fal AI (preferred)

Text-to-image:

```bash
curl -X POST "https://fal.run/openai/gpt-image-2" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "your prompt here",
    "quality": "medium",
    "num_images": 1
  }'
```

Image edit:

```bash
curl -X POST "https://fal.run/openai/gpt-image-2/edit" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "edit description",
    "image_url": "https://...",
    "quality": "medium"
  }'
```

Docs: https://fal.ai/models/openai/gpt-image-2

### Kie.ai (fallback)

Kie.ai exposes the same model with a slightly different API shape. Check current docs at https://kie.ai/docs before calling. The skill should request `gpt-image-2` as the model name and pass an equivalent prompt and quality setting.

---

## Hard rules

- **Never print the API key** in logs, tool output, error messages, or commits.
- **Never commit `.env`** or any file containing a real key. Check `.gitignore` excludes them.
- **Always state cost** before generating.
- **Always wait for approval** before the first generation in a session.
- **Always download the image to disk.** Generated URLs from Fal expire. Save before the URL goes stale.
- **Never invent images locally** or describe an image as "generated" if no API call was made.
