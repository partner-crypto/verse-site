# Verse website

A one-page site in the layout of the Partner site: marketing pages, pricing, FAQ, company pages, legal pages, accounts, plan & billing, and a web version of Verse (Today, Review drills, My verses, Library, Stats).

## What's in this folder

- `index.html`: the whole site: markup, styles and script in one file
- `img/`: phone screens captured from the Verse app
- `data/asv.txt`, `data/kjv.txt`: the full American Standard Version (1901) and King James Version, both public domain, used for search and for any verse people add

## Put it online

The folder is set up for **verse.memorizescripture.app** on GitHub Pages:

- `CNAME` tells GitHub Pages the address, `.nojekyll` makes it serve the files exactly as they are, and `404.html` sends mistyped addresses to the home page.
- In the repository's **Settings → Pages**, deploy from the `main` branch (root folder), set the custom domain to `verse.memorizescripture.app`, and tick **Enforce HTTPS** once GitHub has issued the certificate. `.app` addresses only ever load over HTTPS.
- At Porkbun, under the domain's **DNS**: add a `CNAME` record with host `verse` and answer `partner-crypto.github.io`. To send people who type the bare `memorizescripture.app` to the site, add URL forwarding from `memorizescripture.app` to `https://verse.memorizescripture.app`.

To use another host (Netlify, Cloudflare Pages), upload the folder as-is and delete `CNAME`. Keep `img/` and `data/` next to `index.html`.

With no server behind it, the site runs in **preview mode**, like the Partner site: accounts, plans and verses are simulated in each visitor's browser, and the pages say so. Nothing is charged and no email is sent.

## Make accounts and payments real

At the top of the script in `index.html`:

- `VERSE_API`: the origin of your API ('' means the same origin as the site)
- `SUPPORT_EMAIL`: currently `supportverse@fletcherholding.org`, copied from the app's legal pages
- `LEGAL_ENTITY`: still the placeholder from the app's legal pages

When `GET {VERSE_API}/api/health` returns `{ "api": "verse" }`, the site switches to live mode and sends every call to your server instead. The full list of endpoints and payloads is in the comment block at the top of the script. It covers auth, profile, password, export, delete, checkout, cancel, resume, switch plan, billing portal, contact, and a `/library` document that holds each person's verses and progress.

Payments: `/billing/checkout` should return a hosted checkout URL (Stripe Checkout or RevenueCat Web Billing), and `/billing/portal` a customer-portal URL. The site never handles card numbers.

## Before you launch

- The legal pages still contain the app's placeholders: `[Company legal name]`, `[registered address]`, `[country of incorporation]`, `[governing jurisdiction]` and `[hosting region]`.
- The support address in the app's legal text is `supportverse@fletcherholding.org`, but your domain is `fletcherholdings.org`. Check which one is right, or switch to an address on `memorizescripture.app` (Porkbun can forward it to your inbox for free).
- The Subscription & Billing Terms are written for App Store purchases. Add a section on web checkout before selling on the web.
