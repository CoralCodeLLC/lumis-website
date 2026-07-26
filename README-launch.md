# Lumis Website — lumisapp.org

Files: `index.html` (landing + waitlist form), `privacy.html`, `support.html`, `terms.html`, `CNAME`.
No build step, no frameworks — static HTML. Every page carries its own inline CSS and a small
starfield canvas; there are no external requests at all (no CDNs, no webfonts, no analytics).

## Deployment: GitHub Pages (live)

This repo **is** the live site. `git push origin main` deploys it — Pages serves from the repo root
and `CNAME` holds `lumisapp.org`. Give it 1–2 minutes after a push.

DNS is in Namecheap and already set:

- 4× A record, host `@` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- CNAME, host `www` → `coralcodellc.github.io`
- Email forwarding: `hello@`, `support@`, `privacy@` → Hudson's Gmail

> The old Vercel plan (A record `76.76.21.21`, `npx vercel --prod`) is **superseded** — ignore it.

## Design direction

One committed dark "astral" theme across every page: a fixed starfield behind the whole document,
translucent glass panels, purple accents (`#8B6BFF` / `#B49CFF`), serif display face (Iowan Old
Style / Palatino / Georgia) with a mono face for labels and data. No white grounds anywhere —
that's deliberate, per Hudson, July 2026. Both the OS theme preference and the in-page theme
toggle are pinned to dark so the look never inverts.

## Waitlist form

`index.html` posts JSON to **`POST https://studybuddy-liard-eight.vercel.app/api/waitlist`**
(the `studybuddy` backend in `CoralCodeLLC/Student-Hub`). Fields: `email` (required),
`school` (optional), `betaTester` (bool), `source`, plus a `website` honeypot that real people
never fill in.

Behavior:

- **200 `{ok:true, already:false}`** → success panel replaces the form.
- **200 `{ok:true, already:true}`** → "you were already on it" panel (resubmitting is not an error).
- **429** → too many from one IP (5 per 15 min, 20 per UTC day, counted in Postgres).
- **anything else / network failure** → falls back to a visible `mailto:hello@lumisapp.org` so a
  signup is never silently lost.

Signups land in the `WaitlistSignup` table in Supabase. Export with:

```sql
select email, school, "betaTester", "createdAt" from "WaitlistSignup" order by "createdAt";
```

**Endpoint must be deployed for the form to work.** Until then the form shows its email fallback.
The endpoint ships in the `waitlist-endpoint` branch of `CoralCodeLLC/Student-Hub`; merging it to
`main` applies the schema automatically (the build script runs `prisma db push`).
Optional but recommended: set `WAITLIST_IP_SALT` to a random string in the Vercel project env so
IP hashes aren't guessable.

## App Store dependency

`https://lumisapp.org/privacy.html` and `https://lumisapp.org/support.html` are the required
Support/Privacy URLs for App Store submission. The site must be live before **submission**
(not needed for TestFlight). The privacy policy documents the waitlist data we collect — keep it
accurate if the form fields change.

## Deliberately soft / to revisit

- **Pricing:** no dollar amounts on the site — the pricing decision isn't final. When it is, update
  the Premium card (`#pricing`) and the FAQ answer.
- **Screenshots:** the hero is a CSS phone mockup in the app's dark theme. Swap in real screenshots
  once the UI is final — real screenshots convert better.
- **Terms:** drafted in-house, still wants an attorney pass.
- **Social links:** none in the footer yet — add once the handle is registered.
- **Coral Code studio site:** separate from this repo. Cross-link both ways once its domain exists.
