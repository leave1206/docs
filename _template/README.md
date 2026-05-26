# `apps/docs/_template/` — model docs templates

Authoritative templates for ByteSpike model documentation. Three variants
matching the three production patterns:

| Template | For | Pricing surface |
|---|---|---|
| `model-chat.mdx` | text chat models (OpenAI / Anthropic / Google / DeepSeek / Moonshot / Zhipu / MiniMax) | per 1M input + 1M output tokens |
| `model-image.mdx` | text-to-image models (Seedream / Nano-Banana / GPT-Image) | per image |
| `model-video.mdx` | async video generation (Sora / Veo / Seedance) | per second + submit fee |

## Use

When authoring a new model page, copy the matching template into the
right `apps/docs/api-reference/<family>/` directory and rename to the
model slug:

```bash
cp _template/model-chat.mdx api-reference/text/gpt-5-5.mdx
```

Then fill the `<!-- TODO -->` markers. Keep section order and headings
identical across model pages — the docs are scanned for parity, not for
authorial flourish.

## Source authority

Field spec comes from W1's 2026-05-08 dispatch (`window-3-from-w1-2026-05-08-docs-content.md`).
Field shapes match the four content-rich exemplars on origin/main:

- text: [`api-reference/text/claude-messages.mdx`](../api-reference/text/claude-messages.mdx)
- image: [`api-reference/image/seedream-v4.mdx`](../api-reference/image/seedream-v4.mdx)
- video: [`api-reference/video/sora-2.mdx`](../api-reference/video/sora-2.mdx)
- utility: [`api-reference/utility/balance.mdx`](../api-reference/utility/balance.mdx)

## Voice rules

- **Don't copy aireiter / b.ai prose.** Rewrite for ByteSpike voice
  (concrete, opinionated, second person). SEO duplicate-content penalty +
  ethical sourcing.
- **Lead with what the model does well**, not the marketing tagline. One
  sentence in the description frontmatter; first paragraph of body
  expands.
- **No model versions in body unless they matter.** "Claude Opus 4.7"
  belongs in `model_id`; the body says "Claude Opus" unless the version
  difference shapes a behavior.
- **Cite live pricing, don't hardcode it.** Always link
  `https://bytespike.ai/pricing#<anchor>` instead of pasting a per-token
  number that goes stale.

## Code example contract

Every model page ships **three** examples (curl, Python, Node) for the
canonical request shape. Use the snippets below verbatim and fill in only
the model-specific bits — `model`, request body fields, response fields.
Consistent code shape across pages is what makes the docs feel
trustworthy at scale.
