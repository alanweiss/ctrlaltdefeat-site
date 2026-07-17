# Waitlist form — Formspree setup

The "Notify Me" waitlist form in `index.html` is **fully built** (email field,
`_subject`, `_gotcha` honeypot, and a JS AJAX handler that keeps the visitor on
the page and shows an inline thank-you). It only needs a real Formspree endpoint
ID. Until then the handler gracefully no-ops (shows the thank-you without posting).

## Create the form (~2 min — needs your Formspree account)
1. Go to **formspree.io** → sign up free. Use the inbox where signups should land
   — likely **contact@automatictalent.com** or **geogo.aw@gmail.com**.
2. Verify your email.
3. **New Form** → name it e.g. *"CtrlAltDefeat Waitlist"* → set the notification email.
4. Copy the endpoint — it looks like `https://formspree.io/f/abcdwxyz`.
   The `abcdwxyz` part is the **form ID**.

## Wire it in
- In `index.html` (~line 277), replace `YOUR_FORM_ID` in
  `action="https://formspree.io/f/YOUR_FORM_ID"` with the real ID.
- Delete the stale setup-comment block just above the `<form>` (~lines 270–275).
- Commit, then push to deploy (Cloudflare Pages).

## Activate + test
- Formspree's **first real submission triggers a one-time confirmation email** —
  submit yourself a test signup after wiring to flip the form on.
- Confirm the signup lands in the notification inbox.

## Notes
- **Free tier: 50 submissions/month.** Fine to start; watch it if the waitlist
  takes off (upgrade or swap providers if needed).
- The waitlist collects **email (PII)** — already disclosed in the Privacy Policy
  (`legal/privacy.html` §2 names Formspree; §6 lists it as a service provider).
- Honeypot (`_gotcha`) filters most bot spam automatically; Formspree also has
  its own spam filtering + optional reCAPTCHA in the dashboard.
