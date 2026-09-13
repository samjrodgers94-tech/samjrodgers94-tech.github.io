# Build plan — Sam Rodgers photography portfolio

Hand this file to Claude Code in the repo root. Work through the phases in order and stop for review at the end of each one.

## Context

`index.html` already exists and is the design reference. It is a single self-contained static page: dark cinematic layout, fixed top bar, full-height hero, three collection tiles, filterable masonry gallery, lightbox, About section, Instagram links. **Do not redesign it.** The visual result at the end of this work should be indistinguishable from what's there now.

The owner is a horticulturalist, not a developer. He will add photographs weekly and must never have to edit HTML or JSON by hand after this is done.

Constraints:

- Zero running cost. No paid hosting, no paid services.
- No build step for the site itself. Plain HTML/CSS/JS served statically.
- No framework. Do not introduce React, Astro, Eleventy or a bundler.
- Must work on GitHub Pages.

---

## Phase 1 — Separate content from code

Right now the photo list is a `PHOTOS` array inside a `<script>` tag in `index.html`. Move it out.

Create:

```
content/photos.json
content/site.json
```

`content/photos.json` — an array of objects, same shape as the current `PHOTOS` entries:

```json
[
  { "file": "battleston-01.jpg", "name": "Rhododendron 'Cynthia'", "place": "Battleston Hill", "set": "Battleston Hill", "ratio": "4/5" }
]
```

`content/site.json` — everything that is currently hardcoded copy:

```json
{
  "headline": ["Light.", "Leaf.", "Season."],
  "heroNote": "A year of close looking in the gardens at RHS Wisley...",
  "heroCorner": "A closer look",
  "heroImage": "images/hero.jpg",
  "aboutTitle": "The art of noticing",
  "aboutBody": ["paragraph one", "paragraph two"],
  "instagram": "https://www.instagram.com/samjrodgers_"
}
```

Then in `index.html`, fetch both at load and render. Requirements:

- Use one `fetch` per file, run in parallel, and render only after both resolve.
- `fetch` fails on `file://`, so if the fetch throws, fall back to a small inlined copy of the current data so that opening the file locally by double-clicking still shows something rather than a blank page. Log a one-line console notice explaining why.
- Keep all existing behaviour intact: collection tiles built from distinct `set` values, filter buttons, the `is-hidden` filter mechanism, the dashed placeholder for missing image files, the lightbox with keyboard navigation, the eyebrow built from the first three set names.
- Order of photos in the JSON is the display order. Do not sort.

Verify by serving locally (`python3 -m http.server`) and confirming the page is visually identical to before.

## Phase 2 — Editing without touching code

Wire up **Pages CMS** (`app.pagescms.org`) — open source, free, authenticates with a GitHub App, commits straight to this repo. Chosen over Decap CMS because Decap's usual auth path (Netlify Identity / Git Gateway) is deprecated.

Create `.pages.yml` in the repo root. It needs:

**Media config** pointing at `images/`, with `extensions: [jpg, jpeg, png, webp]`.

**A `photos` collection** of type `list`, file `content/photos.json`, with fields:

| field | type | notes |
|---|---|---|
| `file` | image | must store the filename only, relative to `images/` |
| `name` | string | label it "Plant name", with a hint that it renders in italics |
| `place` | string | label it "Where" |
| `set` | select | options generated from the existing collections, but allow a custom value so new collections can be added |
| `ratio` | select | `4/5`, `3/2`, `2/3`, `1/1`, default `4/5`, marked optional |

Set a `view` with `fields: [file, name, set]` so the list is browsable as thumbnails, and make the list reorderable — drag to reorder must change display order on the site.

**A `site` collection** of type `file`, file `content/site.json`, with the headline as a list of strings (max 3), the About body as a list of strings, and the rest as plain text fields.

Write the exact click-path for connecting the repo to Pages CMS into `README.md` — installing the GitHub App, granting it access to this repo only, and where the editor lives afterwards. Assume the reader has never seen GitHub's app settings.

## Phase 3 — Automatic image resizing

The camera is a 61MP Sony a7CR. Full exports are ~60MB and would make both the repo and the site unusable. The CMS uploads whatever it is given, including straight from a phone, so resizing must happen server-side after upload.

Add `.github/workflows/resize-images.yml`:

- Trigger: `push` on `main`, paths `images/**`.
- Skip runs whose commit author is the workflow's own bot, to avoid an infinite loop.
- Use ImageMagick (available on `ubuntu-latest`) rather than adding a Node dependency.
- For each JPEG/PNG in `images/` whose long edge exceeds **2500px**: resize to 2500px on the long edge, re-encode at quality 82, strip metadata except the colour profile, convert to sRGB.
- Leave anything already under the threshold untouched, so the workflow is idempotent.
- Commit the results back with `[skip ci]` in the message.
- Give the job `permissions: contents: write`.

Test it by committing one oversized file and confirming a single follow-up commit, then no further runs.

## Phase 4 — Deploy

GitHub Pages, `main` branch, root. No Actions-based build needed since there is no build step — use the branch deploy source.

Then in `README.md` document, in plain language and in this order:

1. How to add a photograph using the CMS (the everyday path — this goes first).
2. How to change the headline and About text.
3. How to add a new collection.
4. How to edit files directly in a checkout, for when something needs fixing.
5. The custom domain step, including the `CNAME` file and that a domain bought through Squarespace must be transferred out before that subscription is cancelled.

Keep it short. It is a reference for one non-technical person, not documentation for contributors.

---

## Acceptance checks

- [ ] Page renders identically to the current `index.html`, including the placeholder state for missing images.
- [ ] Deleting an entry from `content/photos.json` removes it from the grid, the tiles and the lightbox with no other change.
- [ ] Adding an entry with a brand-new `set` value creates a fourth tile and a fourth filter button automatically.
- [ ] Reordering entries in the JSON reorders the grid.
- [ ] A 60MB upload ends up under ~1.5MB in the repo, with the page still correct.
- [ ] Lighthouse on mobile: performance and accessibility both above 90.
- [ ] Keyboard alone can reach every filter, open a photograph, move through the lightbox and close it.
- [ ] `prefers-reduced-motion` is still respected.

## Out of scope

Print sales and commission enquiries were deliberately dropped. Do not add contact forms, newsletter signups, analytics, cookie banners or a blog. If a section looks empty, leave it empty.
