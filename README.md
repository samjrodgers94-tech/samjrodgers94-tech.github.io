# Sam Rodgers — photography site

## Turning on GitHub Pages (one-time setup)

Do this first — it's what makes the site visible on the internet at all.

1. On this repository's GitHub page, click **Settings**, then **Pages** in the left sidebar.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Set **Branch** to `main` and the folder to `/ (root)`, then click **Save**.
4. GitHub takes a minute or two to publish. Refresh the Pages settings page and it'll show your site's address — normally `https://<your-github-username>.github.io/<repository-name>/`.

No further setup is needed here — every push to `main` (including the ones the CMS makes for you) updates the live site automatically, with no build step to wait on.

## Connecting Pages CMS (one-time setup)

This lets you edit photos and text through a web page instead of editing files. You only need to do this once.

1. Go to [app.pagescms.org](https://app.pagescms.org) and sign in with your GitHub account (the same account this repository lives in).
2. Click **Add a project** (or **Install GitHub App** if that's what you see first).
3. GitHub will ask which repositories to give Pages CMS access to. Choose **Only select repositories**, then pick this repository from the list. Do not choose "All repositories."
4. Click **Install** (GitHub may ask you to confirm with your password or a security prompt).
5. You'll be dropped back into Pages CMS, now showing this repository. Click into it.
6. Pages CMS reads the `.pages.yml` file in this repo and builds the editor automatically — you'll see **Photographs** and **Site text** in the sidebar.

That's the setup. From now on, editing lives at **app.pagescms.org** — bookmark it. Every change you save there is committed straight to this repository and, within a couple of minutes, goes live on the site.

---

## Adding a photograph (the everyday task)

1. Go to app.pagescms.org and open this site.
2. Click **Photographs**, then **Add**.
3. Click the **Photo** field and upload the image straight from your computer or phone. Don't worry about the size — it gets shrunk automatically after you save (see "Behind the scenes" below).
4. Fill in the plant name, where it was taken, and the collection (type an existing one — e.g. `Woodland` — or a brand new name to start a new collection).
5. Shape can be left as-is; it only matters if you save without a photo attached.
6. Click **Save**. Give it a minute or two, then check the live site.

To change the order photos appear in, drag them up or down in the **Photographs** list. To remove one, open it and delete it.

## Changing the headline or About text

1. Open **Site text** in Pages CMS.
2. Edit the headline lines (up to three short ones), the note under it, or the About paragraphs.
3. Save. Give it a minute, then refresh the site.

## Adding a new collection

You don't need to do anything extra — just type a new name into the **Collection** field on any photograph (see step 4 above). As soon as one photo uses it, it gets its own tile on the homepage and its own filter button. There's no separate place to "create" a collection.

## Editing files directly (for when something needs fixing)

Only needed if the CMS is unavailable or something looks broken:

1. Clone this repository, or edit files directly on github.com.
2. Photos live in `content/photos.json`, site text in `content/site.json` — both plain JSON, one entry per photograph/field.
3. Photo files themselves live in the `images/` folder.
4. Commit and push to `main` — the site updates automatically.

## Custom domain

1. Buy nothing new — if you already have a domain (e.g. one bought through Squarespace), you need to **transfer it out of Squarespace first** — you can't point a domain at GitHub Pages while Squarespace still manages its DNS and keep the Squarespace subscription. Transfer it to a registrar of your choice (or use GitHub's DNS options), then cancel the Squarespace domain subscription once the transfer completes.
2. Add a file named `CNAME` (no extension) to the root of this repository containing just your domain, e.g.:
   ```
   samrodgersphotography.com
   ```
3. At your domain's DNS settings, point it at GitHub Pages following [GitHub's custom domain instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
4. In the repository's Settings → Pages, enter the same domain under "Custom domain" and wait for the certificate to provision (can take up to 24 hours).

---

## Behind the scenes (you don't need to touch this)

- **Image resizing**: a GitHub Action automatically shrinks any photo over 2500px on its long edge and re-compresses it, so a 60MB camera file ends up under ~1.5MB in the repository. This runs a minute or so after you save a photo in the CMS.
- **Deployment**: GitHub Pages serves this site directly from the `main` branch — there's no build step.
