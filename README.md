# aibhlinn.ai, the AibhlínnAI front door

A one-page static site for the bare domain `aibhlinn.ai`: who AibhlínnAI is,
a card for each app (Pie Timers so far), the no-ads promise, a brand kit for
event organisers and media, and a contact address. Plus a `404.html` that
points lost visitors at Pie Timers.

**Home: the `AibhlinnAI/aibhlinn.ai` repo**, published by GitHub Pages
from `.github/workflows/deploy.yml`. It was drafted in `www/` of
`pie-timers-app` and moved here on 1 Oct 2026, because a Pages site has
one custom domain and `pie-timers-app` already uses its one for
`pietimers.aibhlinn.ai`.

## Rules this page keeps

Same principles as the app (`ARCHITECTURE.md` §3 and §4), applied to a page
with far less reason to break them:

- **No JavaScript.** The only `<script>` is JSON-LD, which is data for search
  engines and never runs. Nothing on the page needs code.
- **No third-party requests.** The font is self-hosted (copied from
  `pie-timers-app/app/fonts/`), images are local, and there is no CDN. The
  guard job in `.github/workflows/deploy.yml` scans for secrets and tracker
  domains before every publish, and a hit stops the deploy.
- **No cookies, no analytics.** The footer says so, which makes it a promise.
- **Prices and dates: only the launch offer, and only while it runs.** Since
  22 Nov 2026, the day Pie Timers went public on Google Play, `index.html`
  advertises the Black Friday Launch Offer. It quotes the launch price
  (A$2.90 a month or A$19 a year), the subscribe-by date (5 December 2026),
  the first payment date (1 January 2027), the 100 Foundation places and
  the closing date (the end of 5 December 2026, Adelaide time). This
  replaces the old rule of no prices and no dates, which Mal overruled on
  22 Nov 2026.
  - The same price and dates live in `pie-timers-app`. Its `CLAUDE.md` keeps
    the one list of where, under "The launch dates live in several places".
    That list is not copied here, because a copy went stale within a day.
    `app/foundation.html` is the source of truth. If any of them moves,
    change this page in step, and the other way round.
  - **Publish this page after the app, never before.** The advert links to
    `pietimers.aibhlinn.ai/foundation.html`. Until the `pie-timers-app`
    branch that carries the Black Friday Launch Offer is merged and live,
    that page still describes the October offer. The order is: the SQL, the
    `paddle-webhook` function, the test purchase (Test A), the app pages,
    then this site (`pie-timers-app/DEPLOY.md` 5.6). Open the live
    `foundation.html` and check it says Black Friday Launch Offer before
    merging here.
  - "Pay nothing until 2027" is only true with its condition. Keep it right
    beside the "subscribe by 5 December 2026 (Adelaide time) while your free
    trial is running" sentence, wherever it appears. The offer closes at
    Adelaide midnight, which is earlier on 5 December everywhere to the
    west, so the date always carries "(Adelaide time)".
  - The offer is two things with two closing rules. The date offer (pay
    nothing until 2027) closes at the end of 5 December 2026 (Adelaide
    time) and not before. Foundation (the price lock, 100 places) closes
    then too, or sooner if all 100 places are taken. A full Foundation does
    not end the date offer.
  - The page has no JavaScript, so it cannot count places or hide the offer.
    If Foundation fills before 5 December, remove by hand the Foundation
    paragraph in the offer block of `index.html` and the second sentence of
    its closing paragraph, and leave the rest. When the offer closes, on
    6 December 2026, remove the offer line in the hero and the whole offer
    block in the Pie Timers card. Each is marked with an HTML comment. The
    `.offer` and `.hero-offer` rules in `site.css` can go with them.
  - Outside an offer, link to `pricing.html` instead of quoting a price.
- **No Play steering risk.** The Google Play app never links here. This is
  what lets this page show a price and a dated invitation to subscribe,
  which the app itself must never show inside Google Play. Keep it true: do
  not add a link to `aibhlinn.ai` anywhere the Play app can reach.

## Colours

From `pie-timers-app/brand/README.md`, never invented here. The hero and footer are always
brand blue `#050818`, because the logo artwork has that navy baked in. The
sections between follow the visitor's light or dark setting: blue on white
`#2E3A63` on light, titanium `#CED1D8` (and its gradient) on dark. Pie Timers'
own purple, green and orange appear only inside its card. Every text colour
pair clears 6.9:1 contrast.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The page |
| `404.html` | Served by GitHub Pages for any missing path. Root-absolute links, because it can be served at any depth |
| `site.css` | All styles for both pages |
| `aibhlinn-logo.png`, `aibhlinn-mark-512.png`, `aibhlinn-mark-80.png` | Copies of the files in `pie-timers-app/app/`. The two large ones are the brand-kit downloads |
| `logo.webp`, `logo.jpg` | The logo re-encoded for the hero: 9 KB and 33 KB instead of 361 KB |
| `og-image.jpg` | 1200 × 630 link-preview image (WhatsApp, LinkedIn, Teams, Discord) |
| `favicon.ico`, `apple-touch-icon.png` | Tab and home-screen icons from the mark |
| `CNAME`, `.nojekyll` | The custom domain, and no Jekyll processing |
| `fonts/` | Instrument Sans, copied from `pie-timers-app/app/fonts/`, with its licence |

If the logo artwork ever changes, update `pie-timers-app/app/` and here together, then
regenerate the derived images (ImageMagick):

```bash
convert aibhlinn-logo.png -strip -quality 86 -sampling-factor 4:4:4 logo.jpg
convert aibhlinn-logo.png -strip -quality 84 logo.webp
convert aibhlinn-logo.png -resize 1120x630 -set option:distort:viewport 1200x630-40+0 \
        -virtual-pixel Edge -distort SRT 0 +repage -strip -quality 88 og-image.jpg
convert aibhlinn-mark-512.png -resize 180x180 -strip apple-touch-icon.png
convert aibhlinn-mark-512.png -strip \( -clone 0 -resize 16x16 \) \
        \( -clone 0 -resize 32x32 \) \( -clone 0 -resize 48x48 \) -delete 0 favicon.ico
```

## Previewing

Double-click `index.html`. Everything works from `file://` except the 404
page, whose root-absolute links need a server. The link-preview tags use
absolute `https://aibhlinn.ai/` URLs, so previews only work once it is live.

## Going live, once

1. Repo → **Settings → Pages → Source: GitHub Actions**. The first push to
   `main` then deploys.
2. Cloudflare DNS for `aibhlinn.ai`, all **DNS only (grey cloud)** until
   GitHub has issued the certificate (the lesson from
   `pie-timers-app/DEPLOY.md` §3):
   - four `A` records on the apex `@`: `185.199.108.153`,
     `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - a `CNAME` `www` → `aibhlinnai.github.io`, so `www.aibhlinn.ai`
     redirects to the apex
   - leave the apex `MX` and `TXT` records alone: Cloudflare Email Routing
     and Resend depend on them
3. **Settings → Pages → Custom domain** → `aibhlinn.ai` → Save. When the DNS
   check goes green, tick **Enforce HTTPS**.
