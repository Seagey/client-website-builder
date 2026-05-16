---
name: vercel-deploy
description: Deploy a single static site (a folder containing `index.html`) to Vercel's free Hobby tier and return the live URL. Handles first-run auth setup, CLI install check, and verifies the deploy succeeded. Triggers on "vercel-deploy", "deploy the site", "publish to vercel", "ship it".
---

# vercel-deploy

Deploy a single static site to Vercel and return a live URL. In this kit it is the final phase of `client-website`: after the user approves the built site, this skill puts it live for free.

The deploy lands on a free `.vercel.app` subdomain. No domain purchase required. The user can connect a custom domain later through the Vercel dashboard if they want.

---

## Prerequisites

Before deploying, this skill checks for:

1. **Node and npm.** Run `node --version`. If missing, tell the user how to install Node (https://nodejs.org). Stop until they confirm.
2. **Vercel CLI.** Run `vercel --version`. If missing, run `npm install -g vercel` (or tell the user to run it if the agent cannot install globally).
3. **Authentication.** Run `vercel whoami`. If it errors, the user needs to authenticate. Two options:
   - **Interactive login** (recommended for personal machines). Tell the user: "Run `vercel login` in your terminal. It opens a browser, you sign in once, and from then on the CLI is authenticated."
   - **Token-based** (for remote or SSH setups). Tell the user: "Get a token at https://vercel.com/account/tokens, then either set `VERCEL_TOKEN` in your shell or add it to a project `.env` file. The CLI will pick it up automatically."

Do not attempt to authenticate the user yourself. Auth must happen in their own terminal.

If any prereq is missing, stop and prompt. Do not try to proceed.

---

## Input

The skill expects a folder containing an `index.html` file:

```
sites/{business-slug}/index.html
```

The `{business-slug}` is the lowercase, hyphenated business name. The Vercel project name and the resulting URL will use this slug.

---

## Deploy

Run:

```bash
vercel deploy --yes --prod sites/{business-slug}
```

If `VERCEL_TOKEN` is set in env, the CLI uses it automatically. No extra flag needed.

The output ends with a URL like `https://{business-slug}.vercel.app`. Capture it.

---

## Verify

Before declaring the deploy successful:

```bash
curl -sI https://{business-slug}.vercel.app | head -1
```

Expect `HTTP/2 200` (or `HTTP/1.1 200 OK`). If it returns anything else, the deploy is not really live yet. Wait 5 seconds and retry once. If still failing, surface the actual response to the user instead of pretending it worked.

---

## What to tell the user

After a successful deploy:

> "Live at https://{business-slug}.vercel.app.
>
> That's the free Vercel address. Yours, no hosting bill ever. If you want a custom domain later (`yourbusiness.com.au` or similar), buy one from any registrar, then point it at this Vercel project. Takes about 2 minutes through the Vercel dashboard. Vercel doesn't charge for the domain itself."

Keep it short. Do not pad with marketing language.

---

## Slug rules

- Lowercase
- Hyphens, not underscores or spaces
- No special characters
- Keep it under 40 characters if possible (long Vercel URLs look ugly)

Examples:
- `Sharni Co` → `sharni-co`
- `The Northern Rivers Wellness Studio` → `northern-rivers-wellness`
- `Anya Wells Yoga and Breathwork` → `anya-wells-yoga`

---

## Cost

- Free. Vercel Hobby tier supports unlimited static projects.
- No build process required for these sites (single HTML file). No build minutes consumed.
- Bandwidth on the Hobby tier is generous (100GB per month). A typical static site uses a fraction of that.
- Free tier limits: not for commercial team use. If the user starts using these sites for serious commerce, mention they may want to upgrade to a paid plan eventually.

---

## Common issues

- **"vercel: command not found".** CLI not installed. `npm install -g vercel`.
- **"Error: You're not authorized".** Auth missing or expired. Run `vercel login` again.
- **"Error: Project name already exists".** Someone else's project on Vercel has the same slug. Add a suffix (`sharni-co-au` or `sharni-co-site`) and redeploy.
- **Deploy succeeds but URL 404s.** Wait 10 seconds. Vercel's edge cache can take a moment to propagate. If still 404, the `index.html` is probably in the wrong place. Verify it exists at `sites/{slug}/index.html`.
- **Custom domain not working.** Out of scope for this skill. Direct the user to https://vercel.com/docs/domains.

---

## Hard rules

- **Never deploy without explicit user approval.** Even if everything passed local preview, do not run `vercel deploy` until the user says ship it.
- **Verify the live URL** with curl after deploy. Do not claim success on the basis of CLI output alone.
- **Do not collect or log the user's Vercel token.** It belongs in their shell or `.env`, never in transcripts, file output, or commits.
- **Never delete a Vercel project** without explicit user approval. Even broken deploys keep history that may be useful for rollback.
