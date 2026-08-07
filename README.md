# PhysioAqua

The website for [physioaqua.com](https://physioaqua.com) — an aquatic therapy
clinic in Kharadi, Pune, with a temperature-controlled pool.

It is a **single static page**. No build step, no framework, no package
manager: one `index.html`, one `styles.css`, two self-hosted fonts and four
photographs.

```bash
# preview
python3 -m http.server 8000     # then open http://localhost:8000

# publish
git push origin master
```

Use a local HTTP server rather than opening `index.html` as a `file://` URL —
the map embed and the fonts both behave differently that way.

## How it deploys

GitHub Pages serves this repository at `physioaqua.com` (see `CNAME`) from the
`master` branch. Pushing to `master` publishes. `.nojekyll` is present, so the
files are served exactly as committed with no Jekyll build.

## Layout

| Path | What it is |
|---|---|
| `index.html` | The whole site — markup, copy, and the structured-data block |
| `styles.css` | All styling; custom properties at `:root`, no framework |
| `fonts/` | Outfit and Karla, self-hosted variable woff2 |
| `images/` | Four processed photographs, the favicon, and the logo marks |
| `404.html` | Styled 404, reusing `styles.css` |
| `CHANGELOG.md` | What changed and why |
| `docs/superpowers/specs/` | The design spec behind the current site |

`assets/` is deliberately untracked — it holds the original camera media,
including full-resolution photographs of children. Everything committed here is
served publicly, so only the processed crops in `images/` are published.

Working on this repo? `CLAUDE.md` documents the parts that will bite you.
