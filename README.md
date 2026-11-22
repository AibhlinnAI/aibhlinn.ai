# aibhlinn.ai, the AibhlínnAI front door

A small static site for the bare domain `aibhlinn.ai`. Flat files at the repo
root, no build step:

- `index.html`, the home page: who AibhlínnAI is, a card for each app (Pie
  Timers so far), questions about Pie Timers, the no-ads promise, and a
  contact section with the business details.
- `services.html`, the services page. It mirrors the services page of the
  LinkedIn company page, with examples of our work from `work/`.
- `brand.html`, the brand kit for event organisers and media.
- `privacy.html` and `terms.html`, for this website. Pie Timers has its own,
  on its own domain.
- `accessibility.html`, a plain statement of what has been done and what is
  not perfect.
- `404.html`, which points lost visitors home and at Pie Timers.
- `robots.txt` and `sitemap.xml`.

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
- **No style attributes and no `<style>` blocks.** Every page carries a
  Content-Security-Policy meta tag that blocks them, along with all scripts
  and anything from another host. Put every style in `site.css`.
- **Links out send no referrer.** Every page carries
  `<meta name="referrer" content="no-referrer">`, so a site we link to is not
  told which page the visitor came from. `privacy.html` section 4 says so.
  The trade-off is that other sites cannot see that a visit came from here.
- **One video, and it never starts by itself.** `services.html` has the
  site's only media player: a plain `<video controls preload="none">` with a
  file and a poster from `work/`. Nothing is fetched until the visitor
  presses play, it has no sound, and the page says in words what it shows.
  That page's Content-Security-Policy has `media-src 'self'` added, and no
  other page's does. `accessibility.html` and `privacy.html` section 3 both
  describe the video: keep them true.
- **No cookies, no analytics.** The footer says so, which makes it a promise.
- **No prices and no dates.** Both already live in several places that must
  move together (`CLAUDE.md`, "The launch dates live in several places").
  This page links to `pricing.html` instead of quoting it, so it never goes
  stale on 1 November. If you add a price or a date here, add this repo to
  that list in `pie-timers-app/CLAUDE.md` in the same commit.
- **No Play steering risk.** The Google Play app never links here, and this
  page shows no prices. Keep both true.

## Colours

From `pie-timers-app/brand/README.md`, never invented here. The hero, the
header bar on the inner pages, the "Our promise" band and the footer are
always brand blue `#050818`, because the logo artwork has that navy baked
in. Everything else follows the visitor's light or dark setting: blue on
white `#2E3A63` on light, titanium `#CED1D8` (and its gradient) on dark. Pie
Timers' own purple, green and orange appear only inside its card. Every text
colour pair clears 6.9:1 contrast, and `accessibility.html` says so. A new
text colour must reuse a pair that is already measured (the promise band
reuses the dark theme's), or that line comes off the accessibility page.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The home page |
| `services.html` | The services page. It mirrors the services page of the LinkedIn company page (see Keeping in step with LinkedIn): the short description, the four services, where we work, "Contact us for pricing", four examples of our work, and three links. No price. Its one date is the month of a past event, in a caption, not a launch or offer date |
| `work/` | The four work samples shown on `services.html`, copied unchanged from the files on LinkedIn: three screenshots (PNG, 249 KB, 368 KB and 171 KB), one video (MP4, 25 seconds, no sound, 4.2 MB) and the video's poster (PNG, 408 KB). `services.html` states each size and each picture's width and height. Change the text if a file changes |
| `brand.html` | The brand kit: logo downloads, colours, how to say and write the name, a short description to copy |
| `privacy.html` | Privacy notice for this website. A draft no lawyer has read (see Legal pages) |
| `terms.html` | Terms for this website, with how we may show work we make for a client (`#work`), how to complain (`#problems`) and how to report a security problem (`#security`). A draft no lawyer has read |
| `accessibility.html` | What has been done, what is not perfect, and how to tell us |
| `404.html` | Served by GitHub Pages for any missing path. Root-absolute links, because it can be served at any depth. Its picture is `apple-touch-icon.png`, 25 KB, not the 191 KB mark |
| `robots.txt`, `sitemap.xml` | Allow everything, and list the six real pages. Never list `404.html` |
| `site.css` | All styles for every page. New rules go at the end |
| `aibhlinn-logo.png`, `aibhlinn-mark-512.png`, `aibhlinn-mark-80.png` | Copies of the files in `pie-timers-app/app/`. The two large ones are the brand-kit downloads on `brand.html`, which states their sizes (361 KB and 191 KB). Change the text if the files change |
| `logo.webp`, `logo.jpg` | The logo re-encoded for the hero and the brand-kit preview: 9 KB and 33 KB instead of 361 KB |
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

## Adding a page

1. Copy an inner page such as `accessibility.html` and change the content
   inside `<main>`.
2. Keep the whole head block, in order: charset, viewport, then the three
   policy tags (Content-Security-Policy, referrer, format-detection), then
   the title, description, canonical, theme colour, colour scheme, the three
   icon links, the font preload, `site.css`, and the Open Graph set. Change
   the title, description, canonical, `og:title`, `og:description` and
   `og:url` to the new page's. Keep each `<link>` tag on one line: the guard
   reads them line by line.
3. Keep the skip link, the header bar and the footer exactly as they are.
   The footer is the same on every page. Put `aria-current="page"` on the
   new page's own footer link if it has one, and on its link in the top
   bar if it has one there.
4. Add the page to `sitemap.xml`, and to the footer of every page if
   visitors should find it.
5. No style attributes, no `<style>` block, no script, no form, no media
   player, nothing loaded from another host. The guard refuses all of them.
   The one video is on `services.html`, and the guard allows it only there.

## Legal pages

`privacy.html` and `terms.html` are drafts. **No lawyer has read them**, and
each carries an HTML comment that says so, which the guard checks for. They
say only what is true of this website, claim compliance with nothing, and
give no legal advice. Each has a "Last updated" date: change it with any
change to the words.

They must stay in step with the Pie Timers policies in `pie-timers-app`:

- `privacy.html` mirrors `app/privacy.html` sections 5, 6, 8 and 10: what
  GitHub, Cloudflare, Google and Resend receive, how long email is kept, and
  the 30 days to respond.
- `terms.html` mirrors `app/terms.html` sections 6 and 12: the Australian
  Consumer Law sentence and South Australian law.
- The Questions section of `index.html` restates `app/terms.html` sections 2
  to 6 and `app/privacy.html` section 9. Change them together.
- `accessibility.html` makes claims that can be tested. If a change to the
  site makes one untrue, fix the site or remove the line.
- `terms.html` section 6, "Work we make for you" (`#work`), says we may use
  what we have developed as marketing material, and that anyone identified
  in it can ask for identifiers to be removed. It was added on 22 November
  2026 as a first draft for the owner to approve. Keep its meaning exactly.
  Section 2 of the terms, the second paragraph of `privacy.html` section 7
  (and the last bullet of that page's short version, which points to
  section 7), the paragraph under the examples on `services.html` and the
  last paragraph of that page's "Get in touch" all point to it: change
  them together. They point and do not restate, so the section stays the
  only place that says what we may do.

## Keeping in step with LinkedIn

The LinkedIn company page is `https://www.linkedin.com/company/aibhlinnai/`.
The owner wrote its About text and its services page on 22 November 2026,
and this site was brought into line with them that day. When either
changes, change the other:

- the hero lede, the four "What we do" cards and the Organization
  description in the JSON-LD on `index.html`, and that page's meta and Open
  Graph descriptions, follow the LinkedIn About text
- the "About AibhlínnAI, to copy" paragraph on `brand.html` is the same
  text in the third person, because other people paste it
- `services.html` follows the LinkedIn services page: its opening lines,
  where we work, "Contact us for pricing", the four work samples and their
  captions, and the links
- "We work remotely as standard. If you would prefer us there in person,
  we can travel, and travel costs are invoiced." is the same sentence on
  `index.html`, on `services.html` and on LinkedIn

LinkedIn is linked from the footer of every page, listed in `sameAs` in
the JSON-LD, and named in `privacy.html` section 4. It is a plain link:
no badge, no logo and no widget from LinkedIn.

If the host changes, if email moves to another provider, or if anything
starts counting visits, change `privacy.html` the same day.

## What the deploy guard checks

`.github/workflows/deploy.yml` runs these before every publish, and any
failure stops the deploy:

- no private keys or tokens, and no ad or tracking service named anywhere.
  That search is case-insensitive and reads every file, this README
  included, so never name such a product, even to say it is not used
- the ABN is never written with a plain space before its digits in an HTML
  file. Use `&nbsp;`, or put the letters and the number in separate elements
- no JavaScript: no script tag other than the JSON-LD data block, no event
  handler attributes, no script files
- nothing loaded from another host, no embeds or forms, no style attributes
  and no `<style>` blocks
- no media player, except one video tag on `services.html`. That tag must
  sit on one line and start exactly
  `<video controls preload="none" poster="work/...png" `, with no `autoplay`
  and no `loop`
- every page has the policy tags and a title. `services.html` must have the
  policy with `media-src 'self'` added, and every other page must have it
  without. Every page but `404.html` has a
  description, a canonical address and the footer promise. `404.html` has
  `noindex`. The two legal pages have their "not reviewed" comment
- the JSON-LD parses, every local link, image and video poster points at
  a file that exists, and `sitemap.xml` lists every page except `404.html`

## What the host decides

GitHub Pages serves these files as they are. That sets some limits:

- It cannot send our own response headers, so the policy lives in meta tags
  in each page. A meta tag cannot stop another site framing a page.
- HTTPS is by redirect ("Enforce HTTPS"), not by an HSTS header.
- Files are cached for 10 minutes, so there is no need for `?v=` on
  `site.css`. A change shows within that time.
- There are no server redirects. A page that moves needs a stub left at the
  old address with a meta refresh and a plain link to the new one.
- The domain is **DNS only** (grey cloud) at Cloudflare. Do not switch on the
  Cloudflare proxy without reading `pie-timers-app/DEPLOY.md` 3.1, and change
  `privacy.html` first: it says GitHub serves the pages and names nobody
  else for a visit.

## Merging with the offer branch

The dated launch offer lives on its own branch, `launch-offer-and-play`,
which edits `index.html`, `site.css` and this README. It adds an offer line
in the hero and an offer block in the Pie Timers card, on the home page
only. To keep the two branches merging cleanly:

- In the hero of `index.html`, leave the last line of the lede paragraph
  and the two buttons under it alone. The offer line goes between them.
- Leave the HTML comment above the `#apps` section alone.
- In the Pie Timers card, change nothing between the list of ticks and the
  buttons. The offer block goes between `</ul>` and the buttons.
- In `site.css`, leave `.ticks li::before` to `.next` alone. The offer rules
  go between them. Add new rules at the end of the file.
- In this README, leave the "No prices and no dates" and "No Play steering
  risk" bullets alone, and add nothing directly above or below them.
- This branch itself carries no price, no trial length and no offer date.

## Previewing

Double-click `index.html`. Styles, images and the font all load from
`file://` with the Content-Security-Policy in place. Two things need a
server instead: the 404 page, whose links are root-absolute, and links to
`./` (the mark in the header, and "Contact" in the footer), which open a
folder listing under `file://`. To click between pages, run any static
server in this folder, for example `python -m http.server`, and open the
address it prints. The link-preview tags use absolute `https://aibhlinn.ai/`
URLs, so previews only work once it is live.

## After changing the title, description or image

Chat apps and social sites keep their own copy of a link preview. After a
change to a page's title, description or `og-image.jpg`, old previews stay
until each service fetches the page again. Some offer a tool that forces a
fresh fetch. Otherwise it takes days. Check a new preview by pasting the
link somewhere it has not been posted before.

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
