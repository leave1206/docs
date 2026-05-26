# `apps/docs` — source for docs.bytespike.ai

Mintlify project source. **Edits made here ship to Mintlify cloud
automatically** via `.github/workflows/sync-docs.yml`, which mirrors
this directory into [leave1206/docs](https://github.com/leave1206/docs)
on every push to `main`. Mintlify is wired to that repo and rebuilds
on each commit.

## Local preview

```bash
cd apps/docs
npx mint dev          # http://localhost:3000, hot-reloads on .mdx edits
npx mint broken-links # check internal links
```

`mint dev` reads `docs.json` and renders pages the same way the
production host does.

## Layout

```
apps/docs/
├── docs.json              # Mintlify v4 config (theme, nav, redirects, brand)
├── package.json           # dev + broken-links scripts
├── introduction.mdx       # landing page
├── quickstart.mdx
├── register.mdx
├── top-up.mdx
├── configure-client.mdx
├── authentication.mdx
├── pricing.mdx
├── troubleshooting.mdx
├── claude-code-cli.mdx    # client setup
├── codex-cli.mdx
├── gemini-cli.mdx
├── cursor.mdx
├── migrate-from-openai.mdx
├── migrate-from-anthropic.mdx
├── concepts/              # 14 concept pages (channels, endpoints, billing, …)
├── models/                # per-model deep dives
├── providers/             # per-platform admin documentation
├── api-reference/
│   ├── overview.mdx
│   ├── text/              # Anthropic + OpenAI + Gemini + DeepSeek + …
│   ├── image/             # Seedream / GPT-Image / Nano-Banana
│   ├── video/             # Sora-2 / Veo-3.1 / Seedance
│   ├── utility/           # /v1/models, /v1/balance, /v1/tasks/{submit,query,cancel}
│   └── account/           # /v1/me/{account,billing,usage,webhooks,notifications}
├── employee-onboarding.mdx  # internal (DOSIA-coupled)
├── admin-operations.mdx     # internal (DOSIA-coupled)
└── logo/
    ├── light.svg, dark.svg, favicon.svg
```

## Brand

Colors mirror `packages/ui/src/styles.css` brand-v2 trio:

- mint `#3AEEDE`
- teal-bright `#3AE1E0` (primary)
- cobalt `#3964EF`

Logos are copies of `packages/ui/src/assets/brand/` so they stay in
sync with the marketing site.

## Adding a page

1. Create the `.mdx` file under the right subdirectory.
2. Add the slug to the relevant group in `docs.json`'s
   `navigation.tabs[].groups[].pages` array.
3. (Optional) If the page should be reachable via a short marketing URL
   like `/api/<slug>`, add a redirect entry to `docs.json`'s
   `redirects` block.
4. `git push` to `main` — the sync workflow mirrors the changes, then
   Mintlify rebuilds within ~30s.

## Marketing site integration

`apps/marketing/app/[locale]/docs/[slug]/page.tsx` links externally to
`https://docs.bytespike.ai/api/<slug>` as the "full reference" CTA.
The canonical pages live at `/api-reference/<family>/<slug>`;
`docs.json`'s `redirects` block maps the marketing `/api/<slug>` URL
space to the canonical paths so existing outbound links never 404.

The marketing `/pricing` page links to `https://docs.bytespike.ai/pricing`
for the full multi-model rate card.

## Hosting

- DNS: `docs.bytespike.ai` CNAME → `cname.mintlify.builders`
  (Cloudflare DNS-only)
- TLS: provisioned by Cloudflare custom hostname (DCV via the two TXT
  records under `_acme-challenge.docs` and `_cf-custom-hostname.docs`)
- Edge: Mintlify cloud (Vercel + Cloudflare); no self-hosted build
