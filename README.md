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

- Host: **GitHub Pages**, repo **`alanweiss/ctrlaltdefeat-site`** (public), serving
  branch `main` at root (`/`), HTTPS enforced. This is a SEPARATE repo from the
  game (`ctrlaltdefeat-playtest`) and from the dev repo — its own git remote.
- `~/.local/bin/gh` is the authenticated GitHub CLI (`alanweiss`).

## Deploy / redeploy (GitHub Pages)
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
- **Play link** — currently a disabled "▶ Play — Coming Soon" chip. At launch,
  restore the `Play Now` `<a>` (commented just above it in the `.cta-row`) and
  point its `href` at the final public game URL.
- **Custom domain (GoDaddy)** — see below.
- **Social links** (`<footer>` placeholders) + **prelaunch badge** ("Launching
  Soon" → a date, or remove at launch).
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

## Alternative host — Cloudflare Pages
If you later want faster cache + serverless form handling: Cloudflare dashboard →
**Pages → Connect to Git** → pick this repo, no build command, output `/`. Then
**Custom domains** → enter the domain and add the records it shows in GoDaddy.
Either host serves the *identical* page — the look comes from `index.html`.
