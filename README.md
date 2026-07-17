# CtrlAltDefeat.ai — launch landing page

A single static page in the game's visual style (same palette, fonts, animated
wordmark, globe). Pure HTML/CSS/one tiny JS — no build step.

```
cad-site/
  index.html        ← the page
  assets/
    globe.png       ← hero ball (from "title globe.jpg")
    favicon.png
    shot1.jpg       ← title globe with country patches
    prematch.jpg    ← VS / tournament setup
    inmatch.jpg     ← live match
  README.md
```

## Live site
**https://alanweiss.github.io/ctrlaltdefeat-site/**

- **Launch host = Cloudflare Pages** (decided 2026-07-09 — see the launch plan). Currently
  served from **GitHub Pages** during development; migrate to Cloudflare before launch
  (steps below). Either host serves the *identical* page — the look lives in `index.html`.
- Current dev host: **GitHub Pages**, repo **`alanweiss/ctrlaltdefeat-site`** (public), serving
  branch `main` at root (`/`), HTTPS enforced. This is a SEPARATE repo from the
  game (`ctrlaltdefeat-playtest`) and from the dev repo — its own git remote.
- `~/.local/bin/gh` is the authenticated GitHub CLI (`alanweiss`).

## Deploy / redeploy (GitHub Pages — current dev host)
Edit files in this folder, then from `cad-site/`:
```sh
git add -A
git -c user.name="Alan Weiss" -c user.email="geogo.aw@gmail.com" \
  commit -m "Update landing page"
git push
```
GitHub Pages rebuilds ~1 min after push. The bare URL can cache ~10 min — append
`?v=N` to bust it while checking. (No build step; no serverless functions.)

First-time setup is already done — for reference, it was:
```sh
git init && git add -A && git commit -m "…" && git branch -M main
gh repo create alanweiss/ctrlaltdefeat-site --public --source=. --remote=origin --push
gh api -X POST repos/alanweiss/ctrlaltdefeat-site/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

## Preview locally
Open `index.html` in a browser, or serve it:
```sh
cd cad-site && python3 -m http.server 8080   # → http://localhost:8080
```

## Still to fill in before launch
- **Formspree ID** — the waitlist `<form action="…/f/YOUR_FORM_ID">` is wired for
  Formspree (AJAX submit + `_gotcha` honeypot). Create a form at formspree.io,
  copy its endpoint, and replace `YOUR_FORM_ID` in `index.html`. Until then the
  JS detects the placeholder and falls back to a local thank-you (no email sent).
- **Play link** — the `▶ Play Now` button is now live in the `.cta-row`; set its
  `href` (currently the `FINAL_PUBLIC_URL` placeholder) to the public game URL when
  you turn the game on. (No pre-launch period — pieces go in place, then game on.)
- **Custom domain (GoDaddy)** — see below.
- **Social links** — Twitter/Instagram/TikTok footer placeholders were removed
  (no accounts yet); add them back when handles exist. "Launching Soon" badge
  removed. The waitlist is now a **humans-vs-humans (PvP) heads-up**, not a launch list.
- **SEO meta** — title/description still say "Football (soccer) game" for search;
  soften if you don't want to imply the genre.

## Custom GoDaddy domain (when ready)
1. Add a `CNAME` file to this repo containing just the domain (e.g. `www.yourdomain.com`),
   commit + push. (Or set it in repo **Settings → Pages → Custom domain**, which
   creates the same file.)
2. In **GoDaddy → DNS**:
   - `CNAME` `www` → `alanweiss.github.io`
   - Apex `@`: four `A` records to GitHub Pages IPs
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
3. Back in **Settings → Pages**, tick **Enforce HTTPS** once the cert provisions
   (a few minutes). Propagation is usually minutes, up to ~an hour.

## Launch host — Cloudflare Pages (PRIMARY, decided 2026-07-09)
The launch home. Chosen for a **private** source (GitHub Pages free tier requires a public
repo), a fast global CDN (matters for the large single-file game), custom domain, auto HTTPS,
and serverless form handling available later.

**Migrate before launch:**
1. Cloudflare dashboard → **Pages → Connect to Git** → pick the `ctrlaltdefeat-site` repo;
   **no build command**, output directory `/`.
2. **Custom domains** → enter `ctrlaltdefeat.ai` (and `www`) → add the DNS records it shows
   in **GoDaddy** (replaces the GitHub Pages A/CNAME records above).
3. Confirm HTTPS provisions, then the bare domain serves from Cloudflare.

No rebuild needed — the page is identical; the look comes from `index.html`. Keep the 5 GB
game dev repo (GeoGoClaude) OUT of this repo — deploy only the built landing assets.
Netlify remains a fine fallback if Cloudflare ever doesn't fit.
