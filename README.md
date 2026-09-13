# Sam Rodgers — photography site

**Live at: https://samjrodgers94-tech.github.io/**

This is a static site — no CMS is connected. Photos and text are edited directly on github.com, in plain JSON files. Everything below is written for someone who's never used GitHub's web editor before.

---

## Adding a photograph

Two steps: upload the file, then add one entry describing it.

Not sure which line in `photos.json` matches which photo? Open **[photo-index.html](https://samjrodgers94-tech.github.io/photo-index.html)** — it shows a thumbnail of every photo next to its exact filename, so you can find the right one before editing.

### 1. Upload the photo

1. Go to: **https://github.com/samjrodgers94-tech/samjrodgers94-tech.github.io/upload/main/images**
   (That URL drops you straight into an upload screen for the `images` folder.)
2. Drag your photo in, or click "choose your files."
3. Scroll down to "Commit changes," leave the defaults, and click **Commit changes**.

Don't worry about file size — a GitHub Action automatically shrinks anything over 2500px on its long edge within a minute or two of you uploading it.

### 2. Describe it in the photo list

1. Go to: **https://github.com/samjrodgers94-tech/samjrodgers94-tech.github.io/edit/main/content/photos.json**
   (This opens `content/photos.json` — the list of every photo on the site — in the editor directly.)
2. You'll see a list of entries like this:
   ```json
   { "file": "images/autumn-01.jpg", "name": "Acer palmatum", "place": "RHS Wisley", "set": "Autumn Colour", "ratio": "2/3" }
   ```
3. Copy one whole line (including the `{` and `}` and the comma after it), paste it as a new line anywhere in the list, and change the values:
   - `file` — `images/` followed by the exact filename you uploaded in step 1.
   - `name` — the plant name (shown in italics on the site).
   - `place` — where it was taken.
   - `set` — the collection it belongs to. Use an existing one exactly as spelled elsewhere in the file (currently `Woodland`, `Autumn Colour`, `Detail`), or type a brand new name to start a new collection — it'll get its own tile and filter button automatically.
   - `ratio` — one of `"4/5"`, `"3/2"`, `"2/3"`, `"1/1"`. Only affects the placeholder box shown before the photo loads; safe to leave as-is.
4. Make sure every entry except the last one ends with a comma, and the very last one doesn't.
5. Scroll down, click **Commit changes**, then check the live site in a minute or two.

To remove a photo, delete its whole `{ ... }` line (and its comma if it was the last one). To reorder photos, cut and paste a whole line to a different position in the list — the site displays them in the order they appear here.

## Changing the site's text

1. Go to: **https://github.com/samjrodgers94-tech/samjrodgers94-tech.github.io/edit/main/content/site.json**
2. Edit the text between the quote marks — don't touch the quote marks, colons, or commas themselves.
   - `headline` — up to 3 short lines (the second renders in italics).
   - `heroEyebrow` / `heroNote` / `heroCta` — the small label, subheading, and button text at the top of the page.
   - `heroCaption` — the plant name shown over the corner of the hero photo.
   - `introLabel` / `introQuote` — the "A way of looking" line just below the hero.
   - `aboutTitle` — two short lines for the dark About heading (the second renders in italics).
   - `aboutBody` — the About paragraphs (add or remove lines freely).
   - `aboutGear` — the camera/lens line under the About section.
   - `footerTitle` / `footerBody` — the closing heading and paragraph.
   - `instagram` — your Instagram URL.
3. Commit changes, then check the live site in a minute.

## Custom domain (optional, costs money)

The site currently lives at `https://samjrodgers94-tech.github.io/` — a free GitHub URL (no separate domain purchase). If you'd like a domain like `samrodgersphotography.com` instead, that always requires paying a registrar a yearly fee — there's no free version of this step.

1. If you already have a domain (e.g. bought through Squarespace), **transfer it out of Squarespace first** — you can't point it at GitHub Pages while Squarespace still manages its DNS, and you'd want to cancel the Squarespace domain subscription once the transfer completes.
2. Add a file named `CNAME` (no extension) to the root of this repository containing just your domain, e.g.:
   ```
   samrodgersphotography.com
   ```
3. At your domain's DNS settings, point it at GitHub Pages following [GitHub's custom domain instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
4. In the repository's Settings → Pages, enter the same domain under "Custom domain" and wait for the certificate to provision (can take up to 24 hours).

---

## Behind the scenes (you don't need to touch this)

- **Image resizing**: a GitHub Action automatically shrinks any photo over 2500px on its long edge and re-compresses it, so a 60MB camera file ends up under ~1.5MB in the repository. This runs a minute or so after you upload a photo.
- **Deployment**: GitHub Pages serves this site directly from the `main` branch — there's no build step. Every commit to `main` goes live within a minute or two.
- **A form-based editor (Pages CMS) was considered** instead of editing JSON directly, but signing into it requires granting a third-party app broad `repo`-scope access to your GitHub account (all repos, not just this one) rather than access limited to just this repository, so it was skipped in favor of editing files directly on github.com. The config for it (`.pages.yml`) is still in this repo if you ever want to revisit that trade-off.
