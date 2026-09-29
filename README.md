# Verse website

A one-page site in the layout of the Partner site: marketing pages, pricing, FAQ, company pages, legal pages, accounts, plan & billing, and a web version of Verse (Today, Review drills, My verses, Library, Stats).

## What's in this folder

- `index.html`: the whole site: markup, styles and script in one file
- `img/`: phone screens captured from the Verse app
- `data/asv.txt`, `data/kjv.txt`: the full American Standard Version (1901) and King James Version, both public domain, used for search and for any verse people add

## Put it online

The site lives at **https://versememorizescripture.app** on GitHub Pages (repository `partner-crypto/verse-site`):

- `CNAME` tells GitHub Pages the address, `.nojekyll` makes it serve the files exactly as they are, and `404.html` sends mistyped addresses to the home page.
- In the repository's **Settings → Pages**: deploy from the `main` branch (root folder), custom domain `versememorizescripture.app`, and **Enforce HTTPS** on. `.app` addresses only ever load over HTTPS.
- DNS at Porkbun for `versememorizescripture.app`:
  - `A` records on the bare domain: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
  - `AAAA` records on the bare domain: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
  - `CNAME` record for `www`: `partner-crypto.github.io` (GitHub sends www visitors to the bare domain)

To update the site, replace the files in the repository; GitHub republishes in about a minute. To use another host instead (Netlify, Cloudflare Pages), upload the folder as-is and delete `CNAME`.

## Make accounts and payments real

At the top of the script in `index.html`:

- `VERSE_API`: the origin of your API ('' means the same origin as the site)
- `SUPPORT_EMAIL`: `support@versememorizescripture.app`. Porkbun forwards it to your inbox (Email Forwarding on the domain).
- `LEGAL_ENTITY`: `Partner Limited Liability Company, a Colorado limited liability company`, shown on the Contact page

When `GET {VERSE_API}/api/health` returns `{ "api": "verse" }`, the site switches to live mode and sends every call to your server instead. The full list of endpoints and payloads is in the comment block at the top of the script. It covers auth, profile, password, export, delete, checkout, cancel, resume, switch plan, billing portal, contact, and a `/library` document that holds each person's verses and progress.

Payments: `/billing/checkout` should return a hosted checkout URL (Stripe Checkout or RevenueCat Web Billing), and `/billing/portal` a customer-portal URL. The site never handles card numbers.

## Legal pages

The nine legal documents live in `DATA.LEGAL` in the script, dated by `DATA.LEGAL_UPDATED`. They name Partner Limited Liability Company, a Colorado limited liability company, as the operator; Colorado law governs the Terms; every contact address is `support@versememorizescripture.app`.

- No mailing address is listed. To show one, add it to `LEGAL_ENTITY` and to section 12 of the Terms.
- The Subscription & Billing Terms cover App Store purchases only. Add a section on web checkout before selling on the web.
