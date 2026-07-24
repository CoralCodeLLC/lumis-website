# Lumis Website v2 — lumisapp.org Launch Steps

Files: `index.html` (landing + company story), `privacy.html`, `support.html`.
No build step, no frameworks — static HTML, works from any host. Replaces the v1 site in `Lumis/Website/` on Hudson's Mac.

## Go-live checklist (~20 min total)

1. **Waitlist form (3 min):** create a free form at tally.so (fields: Email / First name / School). Copy the form URL and replace `YOUR_FORM_ID` in `index.html` (one spot, the waitlist button). Signups land in the Tally dashboard and can auto-sync to a Google Sheet.
2. **Deploy (5 min):** from this folder run `npx vercel` then `npx vercel --prod` (any Vercel account; ideally the same one as the backend once Vercel is org-connected). Alternative: push to a `CoralCodeLLC/lumis-website` GitHub repo and import to Vercel for auto-deploy on push.
3. **Domain (5 min, in Namecheap):** Vercel project → Settings → Domains → add `lumisapp.org` + `www.lumisapp.org`. Then in Namecheap → Domain → Advanced DNS:
   - A record, host `@`, value `76.76.21.21`
   - CNAME record, host `www`, value `cname.vercel-dns.com`
   - HTTPS is automatic once DNS propagates (minutes to a few hours).
4. **Email forwarding (5 min, in Namecheap):** Domain → Redirect Email: forward `hello@`, `support@`, `privacy@` → your Gmail(s). The site references all three — do this before sharing the URL so mail doesn't bounce.

## App Store dependency

`https://lumisapp.org/privacy.html` and `https://lumisapp.org/support.html` are the required Support/Privacy URLs for App Store submission (already referenced in the listing package). Site must be live before **submission** (not needed for TestFlight).

## Deliberately soft / to revisit

- **Pricing:** no dollar amounts on the site — Decision Record 4 ($4.99/$29.99 recommended) isn't decided. Once the founders decide, update the Premium card in `index.html` (#pricing) and the FAQ answer.
- **Screenshots:** the hero uses a CSS phone mockup. Swap in real app screenshots once the UI is final — real screenshots convert better.
- **Founder names:** About section uses first names only (Hudson / Jesse). Add last names/photos/links if you want.
- **Social links:** none yet — add to the footer once the handle decision (@getlumis / @uselumis / @lumisplanner) is made and registered.
- **Fonts:** system stacks (Palatino/Iowan serif + system sans) — zero external requests, fast, iOS-native feel. Could swap a webfont later if desired.
