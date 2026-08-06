# PhysioAqua one-page site — design spec

**Date:** 2026-08-06
**Status:** Approved for implementation
**Replaces:** the existing multi-page Jekyll site at physioaqua.com

---

## 1. Goal

Replace the current multi-page Jekyll site with a single page that explains what
aquatic therapy is, establishes PhysioAqua as a credible provider, and gets the
visitor to make contact.

### Audience

Two cold-traffic groups, both of which need educating *and* reassuring:

1. **Patients and family searching Google** — pain, post-surgical recovery,
   arthritis, mobility trouble. Most have never heard of aquatic therapy and do
   not know it is distinct from swimming.
2. **Parents of children with special needs** — paediatric, autism, sensory
   integration. Emotional register is trust and safety; therapist credentials
   matter more than anything else on the page.

Section order follows from this: the page answers *"is this for me?"* before it
asks for anything.

### Non-goals

- No blog, no testimonials, no separate service pages.
- No enquiry form. The site stays fully static; contact is WhatsApp, phone, and
  directions only.

---

## 2. Approach

**Single static page, no build step.** One `index.html`, one stylesheet, no
Jekyll, no Ruby, no bundler, no lockfile. Preview by opening the file; publish by
pushing to `master`.

Chosen over keeping Jekyll because this session surfaced three separate failures
caused by that toolchain — a Windows-only `Gemfile.lock`, a `jekyll-feed` plugin
declared in config but absent from the Gemfile, and a `jekyll-environment-variables`
plugin left over from a Netlify deploy that cannot run on GitHub Pages. At one
page, templating buys nothing that justifies carrying that machinery.

### Target file layout

```
physioaqua.github.io/
├── index.html          # markup + content
├── styles.css          # single stylesheet, no framework
├── CNAME               # physioaqua.com (unchanged)
├── robots.txt
├── sitemap.xml         # single URL
├── .nojekyll           # serve as-is, skip the GH Pages Jekyll build
└── images/
    ├── hero-portrait.jpg
    ├── therapist.jpg
    ├── water-band.jpg
    └── paediatric-band.jpg
```

### Accepted trade-off: loss of search equity

`/services/`, `/team/`, `/contact/`, `/blog/` and six `/services/*` pages have
been indexed since 2023. The clean slate lets them 404. This was raised and
accepted deliberately. Two things partly offset it:

- `MedicalBusiness` structured data (§6) targets the Maps pack, which is where
  "aquatic therapy near me" traffic actually converts.
- The one blog post's substance is absorbed into the "What is aquatic therapy?"
  section rather than discarded.

---

## 3. Page structure

| # | Section | Job |
|---|---------|-----|
| 1 | Hero (split) | State the category, differentiator and location in one line. Primary CTAs. |
| 2 | Is this for you? | Six condition tiles. Both audiences self-identify immediately. |
| 3 | What is aquatic therapy? | The education beat. Absorbs the swimming-vs-therapy blog post. |
| 4 | Why not just any pool? | The founding story, moved out of the buried team page. |
| 5 | Children in the water | Paediatric, autism and sensory work. Serves the second primary audience directly. |
| 6 | Who you'll work with | Dr. Rajashree Fadnavis, credentials, portrait. Highest-stakes section for parents. |
| 7 | Visit us | Address, Plus Code, hours, phone, live map, directions. |
|   | Sticky bar (mobile) | WhatsApp + Call, always within thumb reach. |

### Copy decisions

- **Headline:** *"Physiotherapy that happens in warm water."* Replaces
  *"PhysioAqua — Heal With Water"*, which is poetic but tells a stranger nothing.
  The new line separates the clinic from both swimming and spas in one sentence —
  precisely the confusion the search audience arrives with.
- **Section 4 keeps the first-person plural.** PhysioAqua is a two-founder
  business; "we built this" is accurate. The company speaks as "we"; the
  therapist section speaks in the singular. Ram Shankar Yadav is not featured
  on the page.
- **No hardcoded durations anywhere.** "Practising since 2015" rather than
  "11 years" — a static site cannot update a year count, and the existing
  `_team/rajashree.md` already demonstrates the failure mode, still claiming
  "more than 8 years" from a bio written around 2020.

### Section 5 content (verified)

| Field | Value |
|---|---|
| Name | Dr. Rajashree Fadnavis |
| Role | Co-Founder & Aquatic Physiotherapist |
| Qualification | MPTh, Neuro-Physiotherapy |
| Experience | Practising since 2015 |
| LinkedIn | `https://in.linkedin.com/in/rajashree-fadnavis-606066100` |

The neuro-physiotherapy specialisation is surfaced prominently rather than
buried. It is the credential that most directly reassures a parent bringing an
autistic child, and it justifies the "Neuro rehabilitation" condition tile.

---

## 4. Visual direction — "Water & Light"

| Token | Value | Use |
|---|---|---|
| `--deep` | `#08363F` | Hero ground, dark sections, nav, footer |
| `--mid` | `#0F5A69` | Secondary actions, eyebrow text on light grounds |
| `--lit` | `#7FD8E3` | Primary CTA, stat figures, section rules |
| `--sand` | `#E4D9C6` | Therapist section ground — warm relief from the blues |
| `--paper` | `#F2F8F9` | Conditions section ground |

**Type:** Outfit (200–500) for display, Karla (400–700) for body and UI. Both
must be self-hosted as `@font-face` — no CDN link, which would be a silent
fallback risk.

**Layout:** The hero is a **split**, not full-bleed — type sits on solid
`--deep` at guaranteed contrast while the photo occupies a panel it can fill
well. This is the deliberate fix for photography that cannot carry a full-bleed
hero, and it is the single change that stops the page reading as a snapshot with
words on it.

Accessibility floor, regardless of direction: large tap targets, high contrast,
visible keyboard focus. Visitors are frequently in pain, elderly, or one-handed
on a phone.

---

## 5. Imagery

All images are PhysioAqua's own. No stock.

### Sources and processing

The two strongest images were **not in the JPG files** — they were recovered
from the motion tracks inside the Live Photos (`IMG_1742.MP4`, `IMG_1746.MP4`,
1920×1440), which show the pool with the cover **off** in daylight. The HEIC
files decode grayscale and unrotated through ffmpeg and are unusable.

| Asset | Source | Treatment |
|---|---|---|
| `hero-portrait.jpg` | `IMG_1742.MP4` frame | Crop `1046:1308:560:0` — removes electrical box, storage bins, rolled cover |
| `therapist.jpg` | `IMG_1746.MP4` frame | Crop `1150:1437:79:290` — removes ceiling, pendant light, exhaust fan |
| `water-band.jpg` | `IMG_1742.MP4` frame | Crop `1744:600:0:708` — water surface and refracted limbs, no faces |
| `paediatric-band.jpg` | `IMG_20210307_130527.jpg` (4000×3000, a real photo, not a video still) | Crop `3000:1320:250:560` — framed to both faces and the water moment, child's lower body out of frame |

Grade applied to all three:

```
eq=contrast=1.14:saturation=1.16:brightness=0.012:gamma=1.03,
colorbalance=rs=-0.035:bs=0.055:rm=-0.025:bm=0.035:bh=0.02,
unsharp=5:5:0.55:5:5:0.0,
vignette=PI/5.5
```

### Rejected assets, and why

- **Both wide facility shots** (`IMG_20210126_*`) — pool under its thermal cover,
  shot at 00:43, exhaust fan and clutter in frame. Enlarged in a hero they argue
  directly against the "hospital-clean" claim.
- **Hands-in-water detail** — motion-blurred beyond saving.
- **Shower/changing area** — grades bleak and institutional; undermines the
  hygiene claim rather than evidencing it.
- **Three of the four infant photos** — `IMG_20210307_130636`,
  `IMG_20210307_130727` and `IMG_20210307_132431` are all 16:9 video stills
  (1920×1080, 1920×1080 and 1280×720) and too soft to enlarge. The fourth is
  used; see below.

Strong images with typography carrying the rest beats padding the page with
weak ones.

### Paediatric imagery and consent

Parental consent is confirmed as held for the paediatric photography. One image
is used: `IMG_20210307_130527.jpg`, which — unlike the others in that set — is a
genuine 4000×3000 photograph rather than a video still.

It is framed to the **interaction** rather than the child: both faces, the
therapist's laugh, and the water moment are in frame; the child's lower body is
cropped out. Even with consent held, a healthcare page should show a child as a
participant, not a body.

Implementation requirements:
- Keep the signed consent on file; re-confirm before any future reuse.
- Do not name the child or the parent anywhere on the page or in `alt` text.
- `alt` text describes the activity, not the individuals.

### Outstanding

The library contains no adult patient mid-session, which quietly implies the
clinic mainly treats infants — contradicting five of the six condition tiles.
Not blocking; worth shooting.

---

## 6. Location, contact and SEO

Resolved from the supplied Maps link, matching the Plus Code `HW4P+4M` decode to
five decimals:

- **Coordinates:** `18.5553019, 73.9367421`
- **Place:** `0x3bc2c3df97bf5f9d:0x20550505985dd455`

| Action | Implementation |
|---|---|
| WhatsApp | `https://wa.me/919172777456?text=<prefilled enquiry>` |
| Call | `tel:+919172777456` |
| Directions | `https://www.google.com/maps/dir/?api=1&destination=18.5553019,73.9367421` |
| Map | `https://maps.google.com/maps?q=18.5553019,73.9367421&z=16&output=embed`, `loading="lazy"` |

The coordinate-based directions URL opens the Maps app directly on a phone and
replaces the current `/maps/dir//physioaqua`, which depends on a name search
resolving.

`MedicalBusiness` JSON-LD carrying name, `medicalSpecialty`, geo coordinates,
postal address, telephone and opening hours. With every indexed page being
dropped, this is the primary mechanism buying local visibility back.

---

## 7. Work to discard

The Jekyll repairs made earlier in this session (`_config.yml` `url` and plugin
changes, `Gemfile`/`Gemfile.lock`, `sitemap.xml` rewrite, 404 permalink fix,
deletion of `layout.html` / `analytics.html` / `disqus.html` / `social.html` /
`social.yaml` / `_social.scss`) are **superseded** — every file they repair is
deleted by this rebuild. The working tree should be reset before implementation
begins.

Two changes from that work are worth carrying forward as ideas rather than
diffs: the `permalink: /404.html` discovery (GitHub Pages never found the custom
404 because `permalink: pretty` moved it to `/404/`), and `.superpowers/` in
`.gitignore`.

---

## 8. Definition of done

- [ ] Page renders correctly at 360px, 768px and 1440px with no horizontal scroll.
- [ ] WhatsApp, call, and directions links verified on a real phone.
- [ ] Fonts self-hosted; no external font request.
- [ ] Map iframe lazy-loaded, page usable if it fails to load.
- [ ] `MedicalBusiness` JSON-LD validates in Google's Rich Results Test.
- [ ] Keyboard focus visible on every interactive element.
- [ ] `CNAME` intact; physioaqua.com still resolves after deploy.
- [ ] Paediatric image framed to the interaction; no child or parent named in
      copy or `alt` text; consent record retained.
