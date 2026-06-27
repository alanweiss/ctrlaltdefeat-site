# CtrlAltDefeat.ai — launch landing page

A single static page in the game's visual style (same palette, fonts, animated
wordmark, globe). Pure HTML/CSS/one tiny JS — no build step.

```
cad-site/
  index.html        ← the page
  assets/
    globe.png       ← hero ball (from "title globe.jpg")
    favicon.png
```

## Preview locally
Open `index.html` in a browser, or serve it:
```sh
cd cad-site && python3 -m http.server 8080   # → http://localhost:8080
```

## Things to fill in before launch
- **Play URL** — the hero "Play Now" link currently points at the playtest. Swap
  it for the final game URL.
- **Email capture** — the waitlist `<form>` is a placeholder (shows a thank-you,
  no backend). Wire one of:
  - **Formspree** — set `action="https://formspree.io/f/XXXXX"`, remove the
    `onsubmit`. Emails land in your inbox. Fastest.
  - **Beehiiv / Kit (ConvertKit)** — paste their embed snippet in place of the
    `<form>`. Builds a real mailing list for the launch blast.
- **Screenshots** — replace the 3 `.shot` placeholders with
  `<div class="shot"><img src="assets/shot1.png" alt=""></div>`.
- **Social links** + **prelaunch badge** text ("Launching Soon" → date, or remove
  at launch).

## Deploy (Cloudflare Pages — recommended)
1. Push this folder to a GitHub repo (e.g. `ctrlaltdefeat-site`).
2. Cloudflare dashboard → **Pages → Create → Connect to Git** → pick the repo.
   Build command: *(none)*. Output dir: `/`.
3. **Pages → Custom domains → Set up a domain** → enter your GoDaddy domain.
   Cloudflare gives you the DNS records to add.
4. In **GoDaddy → DNS**, add the records Cloudflare shows (a `CNAME`/`A` for the
   root + `www`). Propagates in minutes; SSL is automatic.

### Or GitHub Pages (simplest, you already use it)
Push to a repo, enable Pages (branch `main`, root). Then GoDaddy DNS: `CNAME`
`www` → `<user>.github.io`, and four `A` records for the apex to GitHub's IPs
(185.199.108–111.153). Note: ~10-min cache, no serverless functions.

Either host serves the *identical* page — the look comes from `index.html`, not
the host. Cloudflare just adds faster cache + optional Functions for forms.
