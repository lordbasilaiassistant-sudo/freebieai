# 🎨 FreebieAI — Free AI Image Generator (No Key, No Signup, No Paywall)

A single static `index.html` you can drop on **GitHub Pages** (or any static host) that lets
**anyone** generate AI images in their browser — **no API key to paste, no signup form, no paywall.**

**Live:** https://lordbasilaiassistant-sudo.github.io/freebieai/

Every model in the UI was tested and confirmed to run **anonymously, with zero credits.**
*(Re-verified working 2026-08-07 — FLUX schnell generated anonymously, no dialog, ~9s.)*

> **More free, no-signup tools from the same tiny lab:** [broke2builtai.com/tools](https://broke2builtai.com/tools/) —
> plus [CoverForge](https://lordbasilaiassistant-sudo.github.io/coverforge/) (covers),
> [ThumbForge](https://lordbasilaiassistant-sudo.github.io/thumbforge/) (YouTube thumbnails),
> [Aeon](https://lordbasilaiassistant-sudo.github.io/aeon/) (a browser god-game where every creature is a real neural net).

---

## What it does

- Prompt → image, with **6 free models**, **aspect-ratio presets**, and **1 / 2 / 4** images per run
- 🎲 **Surprise me** example prompts, **Ctrl/Cmd+Enter** to generate
- Click any result to open a **lightbox**; **Save** per image or **Download all**
- A **session gallery** of everything you've made
- 100% client-side — your prompt goes straight from your browser to the model provider

| Model | Provider | Notes |
|-------|----------|-------|
| `black-forest-labs/flux-schnell` | FLUX | fast |
| `black-forest-labs/flux-1.1-pro` | FLUX | best quality |
| `gpt-image-1`, `gpt-image-2` | OpenAI GPT-Image | returns data-URL PNGs |
| `stabilityai/stable-diffusion-3-medium` | Stability | |
| `stabilityai/stable-diffusion-xl-base-1.0` | Stability (SDXL) | |

---

## How it's free, with no key and no signup

Generation runs through [**Puter.js**](https://developer.puter.com), a client-side SDK with a
**"User-Pays"** model:

- The site needs **no API key and no backend** — just one `<script>` tag.
- On first use the visitor taps **"Continue"** once on Puter's consent dialog; an **anonymous
  account is created automatically** (no email, no password, no form). Light usage is free.

---

## Why image-only (the honest part)

I tested the video side too. **Free, keyless, no-signup *generative* video does not exist in a
browser as of June 2026:**

- Every Puter video model (Veo, Sora, Wan, Seedance, Vidu) returns **`Insufficient funds`** — paywalled.
- **Pollinations** went paywalled (HTTP `402` x402 crypto-gate), confirmed via curl *and* in-browser.
- **Hugging Face** ZeroGPU spaces return `event: error` without an auth token.
- **Craiyon** is `403` Cloudflare-blocked.

Rather than ship a paywalled tease or a fake "video" effect, the tool does one thing well: **free
image generation that actually works.** Video can be added if a genuinely free provider appears.

---

## Deploy to GitHub Pages (2 minutes)

1. Put `index.html` (+ `.nojekyll`) in a repo root and push.
2. Repo → **Settings → Pages → Deploy from branch → `main` / root** → Save.
3. Live at `https://<user>.github.io/<repo>/`.

No build step, no env vars, no secrets — it's one file.

---

## Verified API call shape (for reference)

```js
// Returns an <img>. GPT-Image returns a data: URL; FLUX/SD return hosted URLs.
await puter.ai.txt2img({ prompt, model: 'black-forest-labs/flux-schnell', width: 1024, height: 1024 });
```

> The bare `txt2img(prompt, true)` form in some Puter docs is outdated (errors `Missing model`).
> Always pass the options object with an explicit, valid `model`. Note: `dall-e-3`, `gemini`, and
> `grok-2-image` are **not** valid txt2img model strings — they 404.

## Local dev

```bash
python -m http.server 8747   # → http://localhost:8747/index.html
```

## License

Public / do-whatever. A free tool for everyone.
