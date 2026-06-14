# 🎨 FreebieAI — Free Image & Video Generator (No API Key, No Signup)

A single static `index.html` you can drop on **GitHub Pages** (or any static host) that lets
**anyone** generate AI **images and videos** in their browser — with **no API key to paste** and
**no signup form**.

Live test result (FLUX schnell, generated through the UI, anonymous): a real 1024×1024 image in ~6s.

---

## How it works

All generation goes through [**Puter.js**](https://developer.puter.com), a client-side SDK that uses a
**"User-Pays"** model:

- The page builder (you) needs **no API key and no backend** — just include one `<script>` tag.
- The end user clicks **"Continue"** once on Puter's consent dialog; an **anonymous account is created
  automatically** (no email, no password, no form). Light usage is free.
- Requests go **straight from the browser to the model provider** — no server of ours in the middle.

This gives a genuinely multi-provider stack behind one keyless library:

| Type  | Models available in the UI |
|-------|----------------------------|
| Image | FLUX schnell / 1.1 pro · DALL·E 3 · GPT-Image 1/2 · Gemini · Grok 2 Image · Stable Diffusion 3 · SDXL |
| Video | Veo 3.1 (fast) · Veo 3.0 / 2.0 · Sora 2 / 2 pro · Wan 2.2 (14B) · Seedance 1.0 lite/pro · Vidu Q1 |

**Test mode** toggle returns a free sample (no credits used) so you can demo the flow — on by default
for video, since video is the heavier operation.

---

## Deploy to GitHub Pages (2 minutes)

1. Create a repo (e.g. `freebieai`) on the GitHub account you actually control.
2. Add `index.html` to the repo root and push.
3. Repo → **Settings → Pages → Build from branch → `main` / root** → Save.
4. Your tool is live at `https://<user>.github.io/freebieai/`.

No build step, no env vars, no secrets. It's one file.

---

## Provider research notes (evidence, June 2026)

Tested keyless image/video sources before building so this ships on something that actually works:

- **Pollinations** (`image.pollinations.ai`) — **now gated.** Returns HTTP `402` with the `x402`
  crypto-micropayment protocol for fresh prompts; confirmed blocked both via `curl` and in-browser
  (real `Origin`/`Referer`). The "free, no key" marketing pages are stale. **Not used.**
- **Hugging Face ZeroGPU Spaces** (e.g. FLUX.1-schnell) — Gradio 5 API reachable at
  `/gradio_api/call/infer`, but inference returns `event: error` without an auth token now
  (ZeroGPU quota requires login). **Not reliable keyless.**
- **Craiyon v3** — `403` (Cloudflare-blocked to scripted callers).
- **Puter.js** — ✅ works keyless and signup-form-free; real generations verified for both
  `puter.ai.txt2img()` and `puter.ai.txt2vid()`. **This is what FreebieAI uses.**

### Verified API call shapes

```js
// Image — returns an <img> whose src is a hosted URL (or data URL)
await puter.ai.txt2img({ prompt, model: 'black-forest-labs/flux-schnell', width: 1024, height: 1024 });
await puter.ai.txt2img({ prompt, model: 'dall-e-3', test_mode: true }); // free sample

// Video — returns a <video> element with controls
await puter.ai.txt2vid({ prompt, model: 'veo-3.1-fast-generate-preview', test_mode: true });
await puter.ai.txt2vid({ prompt, model: 'sora-2', seconds: 6 });
```

> Note: the bare `txt2img(prompt, true)` form shown in some Puter docs is outdated — the current API
> errors with `Missing model`. Always pass the options object with an explicit `model`.

---

## Local dev

```bash
python -m http.server 8745
# open http://localhost:8745/index.html
```

## License

Public / do-whatever. Built as a free tool for everyone.
