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
  (A$2.90 a month or A$19 a year), the first payment date (from 1 January
  2027), the 100 Foundation places and the closing date (the end of
  31 December 2026, Adelaide time). This replaces the old rule of no prices
  and no dates, which Mal overruled on 22 Nov 2026. Mal revised the offer's
  terms the same day: the hook lost its subscribe-by date, and the close
  moved from the end of 5 December to the end of 31 December.
  - The same price and dates live in `pie-timers-app`. Its `CLAUDE.md` keeps
    the one list of where, under "The launch dates live in several places".
    That list is not copied here, because a copy went stale within a day.
    `app/foundation.html` is the source of truth. If any of them moves,
    change this page in step, and the other way round.
  - **Publish this page after the app, never before.** The advert links to
    `pietimers.aibhlinn.ai/foundation.html`. Until the `pie-timers-app`
    branch that carries the Black Friday Launch Offer is merged and live,
    that page still describes the October offer. This site goes last, after
    everything in `pie-timers-app/DEPLOY.md` 5.6, which keeps the one list
    of steps and their order (it is not copied here, because a copy goes
    stale). Open the live
    `foundation.html` and check it says Black Friday Launch Offer before
    merging here.
  - "Pay nothing until 2027" is only true with its condition. Keep it right
    beside the sentence "Subscribe while your free trial is running and
    your first payment is taken from 1 January 2027", wherever it appears.
    Always "from 1 January 2027", never "on": the first payment is taken at
    12:00 UTC on 1 January, which is early on 2 January in New Zealand.
    The hook has no subscribe-by date. "Subscribe by 5 December" was its
    condition for a few hours on 22 Nov 2026 and Mal took it off: do not
    put it back.
  - The offer is two things. The hook (pay nothing until 2027) is true for
    anyone who subscribes while their free trial is running, and stays on
    this page to the end of 31 December 2026 (Adelaide time). Foundation
    (the price lock, 100 places) closes at the end of 31 December 2026
    (Adelaide time), or sooner if all 100 places are taken. A full
    Foundation does not end the hook. The close is Adelaide midnight, which
    is still 31 December everywhere to the west, so the date always carries
    "(Adelaide time)".
  - The page has no JavaScript, so it cannot count places or hide the offer.
    It does not need to: **the advert retires itself**, and nobody has to be
    available on 1 January 2027. See "The advert retires itself" below.
  - The hero line, the heading of the offer block and the Foundation
    paragraph each say which day the offer ends ("Until the end of
    31 December 2026 (Adelaide time): ..."), and nothing in the block goes
    false if it lingers for any reason: the hook and its condition are
    about the first payment, and that rule has no cut-off. Keep it that
    way: never write "on now", "this month" or "ends soon".
  - 5 December 2026 is not on this page and must not be. The only thing
    that still ends then is the free trial of a Pie Timers account made
    before 22 November 2026, which the app's own pages explain.
  - Outside an offer, link to `pricing.html` instead of quoting a price.
- **No Play steering risk.** The Google Play app never links here. This is
  what lets this page show a price and a dated invitation to subscribe,
  which the app itself must never show inside Google Play. Keep it true: do
  not add a link to `aibhlinn.ai` anywhere the Play app can reach.

## The advert retires itself

Mal asked for this on 22 Nov 2026: nothing about the offer may need a
person on 1 January 2027.

**How.** In `index.html` the advert sits between marker comments, each on a
line of its own:

- `<!-- OFFER:START -->` and `<!-- OFFER:END -->`, three times: around the
  offer line in the hero, around a note to maintainers above the apps
  section, and around the offer block in the Pie Timers card.
- `<!-- FOUNDATION:START -->` and `<!-- FOUNDATION:END -->`, once, inside
  the offer block: around the Foundation paragraph. Everything the advert
  says about Foundation is in that one paragraph.

`.github/workflows/retire-offer.yml` runs on a schedule: every hour from
13:40 UTC on 31 December to the end of 3 January, then once a day for the
rest of January. Each run reads the clock itself. Before the end of
31 December 2026, Adelaide time (the instant `2026-12-31T13:30:00Z`), it
does nothing. From then on it deletes everything from each START marker to
its END marker, markers included, commits that to `main` as
`github-actions[bot]`, and starts the Deploy workflow. Once the markers are
gone there is nothing to do, so every later run ends cleanly.

**Why it starts the deploy itself.** `deploy.yml` runs on a push to `main`,
but a push made with a workflow's own token does not trigger other
workflows. So the retire workflow runs `gh workflow run deploy.yml`, which
GitHub does allow, and which `deploy.yml` already accepts (it is the
"redeploy by hand" button). The deploy runs exactly as it does for a
person, guard job included. `deploy.yml` itself is not changed.

**It refuses rather than half-do the job.** If a marker is missing its
partner, shares its line with anything else, or sits around half an
element, the run fails, the file is left exactly as it was, and the run
goes red in the Actions tab. The page is checked to be well formed after
the removal before anything is committed.

**To run it by hand**: GitHub, this repo, **Actions**, **Retire the launch
offer advert**, **Run workflow**, branch `main`. Two tick boxes:

- **foundation_only**: all 100 Foundation places have gone before the end
  of 31 December. Takes down only the Foundation paragraph, now. The rest
  of the advert stays, because a full Foundation does not end "Pay nothing
  until 2027". It comes down by itself when the offer closes.
- **force**: take the whole advert down now, whatever the date.

With neither box ticked, a hand run does what a scheduled run would do at
that moment: nothing before the close, the whole advert after it.

**What has to be true for it to work.** Check these before relying on it:

- The workflow file is on `main`. GitHub only runs schedules from the
  default branch.
- The repo has had a commit in the last 60 days. GitHub switches scheduled
  workflows off in a repo that has been quiet for longer. Merging this
  branch in late November 2026 is inside that window.
- `main` accepts a push from `github-actions[bot]`. If a branch protection
  rule requires a pull request, the push step fails and the advert stays
  up. That is the case the self-dating wording is there for.
- Nobody has moved or reworded a marker. They must stay exactly as written.

**Afterwards.** The `.offer` and `.hero-offer` rules in `site.css` style
nothing once the advert is gone. Delete them, and this section, whenever
someone is next here. No date forces it. The workflow file can go at the
same time.

**If the closing day moves**, change `CLOSES` and the cron lines in
`retire-offer.yml` and the wording in `index.html`, together with
`pie-timers-app`.

**Rehearsing it.** The step's Python can be run on a copy of `index.html`
with the clock overridden, for example
`RETIRE_OFFER_NOW=2026-12-31T13:30:00+00:00 python3 retire.py index.html`
after copying the script out of the workflow's heredoc. The workflow itself
never sets `RETIRE_OFFER_NOW`. Never rehearse on the real file.

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
| `.github/workflows/deploy.yml` | Publishes the site on every push to `main`, after the guard job |
| `.github/workflows/retire-offer.yml` | Takes the launch offer advert out of `index.html` after 31 December 2026. Not published: the deploy leaves `.github` out |
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
