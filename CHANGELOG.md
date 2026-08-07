# Changelog

Notable changes to physioaqua.com. Newest first.

The site is a single static page deployed from `master` via GitHub Pages; every
entry here is something visitors saw change. Format loosely follows
[Keep a Changelog](https://keepachangelog.com/).

---

## 2026-08-07

### Fixed

- Two text colours failed the WCAG AA contrast minimum (4.5:1) and cost four
  points in the Lighthouse accessibility audit:
  - `#5E7C82` on white measured **4.49** — missing by 0.01 — used in the
    condition tiles and the "why not just any pool?" cards.
  - `#7C6A50` on the sand ground measured **3.73**, used for the
    "Who you'll work with" label. Bold type only qualifies for the relaxed
    3:1 threshold from 18.66px up; that label is 11px, so it needed the full
    4.5:1.

  Both became named tokens with real headroom — `--muted` (5.62:1) and
  `--muted-warm` (5.55:1) — rather than raw hex repeated across rules. A
  comment records that they may only ever be darkened; drifting them lighter
  is exactly how a 4.49 happens.

- The hero copy was full-bleed while every other section sat in the 1180px
  content column, so on wide viewports the headline drifted away from the
  page's text column. The hero grid now shares that column; the dark ground
  still bleeds full width behind it.

- The paediatric photograph was being upscaled past its native 1600px and
  re-cropped by `object-fit: cover`, which sliced both faces out of frame.
  The crop is already final, so the page now displays it whole.

### Added

- `<lastmod>` in `sitemap.xml`. Google uses it as a freshness signal when it
  looks trustworthy, and it was missing at exactly the moment the whole site
  changed.
- This changelog, and a README describing what the repository actually is.

---

## 2026-08-06

### Changed

- **Rebuilt the site as a single static page.** One hand-written
  `index.html` and one stylesheet. No Ruby, no bundler, no build step;
  `.nojekyll` tells GitHub Pages to serve the files verbatim.

  The previous Jekyll site had accumulated three separate toolchain faults: a
  `Gemfile.lock` locked to `x64-mingw32` only, a `jekyll-feed` plugin declared
  in `_config.yml` but absent from the Gemfile, and a
  `jekyll-environment-variables` plugin left over from a Netlify deploy that
  cannot run on GitHub Pages at all. At one page, none of it earned its keep.

- Page order now follows the two audiences it serves — people searching for
  help who have never heard of aquatic therapy, and parents of children with
  sensory needs. It answers "is this for me?" before asking for anything:
  hero, conditions, what aquatic therapy is, why this pool, children,
  therapist, visit.

- Headline changed from "Heal With Water" to "Physiotherapy that happens in
  warm water", which separates the clinic from both swimming and spas in one
  line — the confusion most visitors arrive with.

- The founding story ("our patients kept stopping") moved out of the buried
  team page into the centre of the argument.

- Experience is stated as "Practising since 2015" rather than a year count.
  The previous site demonstrated why: its bio still claimed "more than
  8 years" six years after it was written.

### Added

- `MedicalBusiness` structured data carrying the clinic's verified
  coordinates, hours, address and telephone — the main mechanism for local
  search visibility now that the previously indexed URLs are retired.
- Self-hosted Outfit and Karla as variable woff2 (56KB for both), replacing
  the Google Fonts CDN request and its silent-fallback risk.
- WhatsApp as the primary contact action, with a prefilled enquiry message.
- A sticky WhatsApp/Call bar on mobile.
- Coordinate-based directions (`?api=1&destination=18.5553019,73.9367421`),
  replacing a name-search URL that depended on "physioaqua" resolving.
- A styled 404 page, with `permalink: /404.html` — under the old Jekyll
  config, `permalink: pretty` had moved it to `/404/`, so GitHub Pages never
  found it and served its own generic page instead.

### Removed

- The multi-page Jekyll site: collections for services, team and
  testimonials, a partially-imported Bootstrap 4 Sass tree, a Netlify-era
  analytics plugin, and placeholder testimonial content from the original
  template. `/services/`, `/team/`, `/blog/` and `/contact/` now 404 — a
  deliberate trade, accepted with the loss of their search equity.
- Google Analytics, which had never worked: the include was gated on a
  Netlify environment variable that GitHub Pages cannot provide, with a
  fallback to a null ID, and a `'{{ $gid }}'` template literal that was not
  valid Liquid.
- Original camera media from version control (`assets/` is now ignored). It
  includes full-resolution photographs of children, and everything committed
  here is served publicly. Only the processed crops in `images/` are
  published.

---

## Before 2026-08-06

The repository held a multi-page Jekyll site, largely unchanged between 2020
and 2023. `git log` before `e01b029` has that history; the reasoning for
retiring it is in
`docs/superpowers/specs/2026-08-06-one-page-site-design.md`.
