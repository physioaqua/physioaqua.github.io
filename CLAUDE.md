# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The PhysioAqua website (aquatic therapy clinic, Kharadi, Pune): a **single static page**, no
build step. Deployed as a GitHub Pages organization site — `physioaqua/physioaqua.github.io`,
served from `master` at `physioaqua.com` (`CNAME`). Pushing to `master` publishes.

`.nojekyll` is present, so GitHub Pages serves the files verbatim and does not run a Jekyll
build. There is no Ruby, no bundler, no lockfile, and no package manager of any kind.

```bash
# preview
xdg-open index.html          # or: python3 -m http.server 8000

# publish
git push origin master
```

Preview through a local HTTP server rather than `file://` when checking the map embed or
fonts — both behave differently over `file://`.

## Structure

| Path | Purpose |
|---|---|
| `index.html` | The entire site: markup, copy, and the JSON-LD block |
| `styles.css` | All styling. Custom properties at `:root`, no framework |
| `fonts/` | Outfit and Karla as self-hosted variable woff2 |
| `images/` | Four processed photographs plus the favicon |
| `404.html` | Styled 404, reuses `styles.css` |

The page is seven sections in a fixed narrative order — hero, conditions, what aquatic therapy
is, why this pool, children, therapist, visit. That order is deliberate: both target audiences
arrive cold, so the page answers "is this for me?" before it asks for anything. Reordering
sections undoes the argument.

## Things that will bite

- **Fonts are variable woff2** declared once each with `font-weight: 100 900`. Do not add
  per-weight `@font-face` blocks; there is one file per family. Never swap these for a Google
  Fonts `<link>` — a CDN request reintroduces a silent-fallback risk and a third-party
  dependency the site otherwise does not have.
- **Contact details are repeated** across the header, hero, visit section, footer, and sticky
  mobile bar. The phone number appears in `tel:` links, the WhatsApp `wa.me` URL, and the
  JSON-LD. Change one, grep for the rest.
- **The `wa.me` link carries a prefilled message.** Keep it URL-encoded.
- **Directions use coordinates**, not a name search:
  `?api=1&destination=18.5553019,73.9367421`. This resolves reliably where the old
  `/maps/dir//physioaqua` did not.
- **JSON-LD must stay in sync with the visible page.** Hours, address, phone and geo are
  duplicated there. Divergence is a structured-data error, and this markup is what feeds the
  Google Maps pack — the main source of local traffic since the previous multi-page site's
  indexed URLs were retired.
- **No hardcoded durations.** Copy says "Practising since 2015", never a computed year count;
  the footer year is set by script. A static page cannot update itself, and the previous site
  demonstrated the failure — its bio still claimed "more than 8 years" six years on.

## Imagery

Every image is PhysioAqua's own; no stock. Source photographs and the exact ffmpeg crop and
grade used to produce each one are recorded in
`docs/superpowers/specs/2026-08-06-one-page-site-design.md` §5. Re-derive from that spec rather
than re-cropping by eye, so the grade stays consistent across the set.

The paediatric photograph is published with parental consent on file. Do not name the child or
the parent in copy or `alt` text, and re-confirm consent before using any further paediatric
imagery.

**`assets/` is gitignored on purpose.** It holds the original camera media, including
full-resolution photographs of children. Everything committed to this repository is served
publicly from physioaqua.com, so the originals stay local and only the processed crops in
`images/` are published. Do not commit `assets/`, and do not "fix" the ignore rule.

## History

The repository previously held a multi-page Jekyll site (collections for services, team and
testimonials, a partially-imported Bootstrap 4 Sass tree, and a Netlify-era analytics plugin).
It was removed wholesale in the rebuild. `git log` before the rebuild commit has it if needed;
the reasoning for dropping it is in the spec.
