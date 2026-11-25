# Mount & Craft website

The static website for **Mount & Craft**, including the player's guide. There is no build step and no framework: every page is a plain HTML file, and Netlify serves the folder as it is.

## What is in here

| Path | Page |
|---|---|
| `index.html` | Home page: the modpack, how to install it, the kingdoms and the server |
| `guide/index.html` | Player's guide (version 1.2.5), with its own sticky contents, search and fourteen chapters |
| `guide/img/` | Screenshots used by the guide |
| `wiki.html` | Wiki and FAQ |
| `privacy.html`, `terms.html` | Legal pages, sharing `legal.css` |
| `404.html` | Not-found page (Netlify uses it automatically) |
| `img/` | Home page artwork (`.webp`) |
| `favicon.svg`, `favicon-32.png`, `apple-touch-icon.png` | Site icons |
| `robots.txt`, `sitemap.xml` | Search engine files |
| `netlify.toml` | Publish settings, extensionless URL rewrites, cache and security headers |

The home page, wiki and legal pages share the same type and palette (Alegreya, Jacquard 24 and Silkscreen, loaded from Google Fonts). The guide carries its own inline stylesheet, so it stays self-contained apart from the fonts.

## Preview it locally

From this folder:

```sh
python -m http.server 8080
```

Then open <http://localhost:8080/>. The guide is at <http://localhost:8080/guide/> (keep the trailing slash in a plain file server). The wiki and legal pages are served as <http://localhost:8080/wiki.html>, `/privacy.html` and `/terms.html` locally; Netlify also answers `/wiki`, `/privacy` and `/terms` through the rewrites in `netlify.toml`.

## Put it on GitHub

```sh
git init
git add .
git commit -m "Mount & Craft site with the player's guide"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git push -u origin main
```

The repository root is the website root, so `index.html` must stay at the top level (do not wrap everything in an extra folder).

## Connect it to Netlify

1. Sign in at <https://app.netlify.com> and choose **Add new site → Import an existing project**.
2. Pick **GitHub**, authorise Netlify if asked, and select the repository you just pushed.
3. Netlify reads `netlify.toml`, so leave **Build command** empty and keep **Publish directory** as `.`.
4. Choose **Deploy site**. The first deploy takes a few seconds; every later push to `main` redeploys automatically.

Then, under **Domain management**, add `mountandcraft.com` and point the domain's DNS at Netlify as the panel instructs. Netlify issues the HTTPS certificate automatically.

### If you deploy without netlify.toml

Netlify's default "Pretty URLs" serves `/wiki` from `wiki.html` and `/guide` from `guide/index.html`. Keep it on, or keep the rewrites in `netlify.toml`, otherwise the navigation links will 404.

## Editing the guide

- All guide text is in `guide/index.html`, chapter by chapter. Each chapter is a `<section class="ch" id="...">` with a `data-title`; the contents list, the reading-position marker and the "Find a topic" filter are built from those sections by the small script at the bottom of the page.
- Screenshots live in `guide/img/` and are referenced as `/guide/img/<name>.jpg`. Keep that absolute path, so the images work whether the page is served at `/guide` or `/guide/`.
- If you add or rename a top-level page, update `sitemap.xml` and the navigation in each page's `<nav>`.

## Notes

- **Canonical domain:** the `<link rel="canonical">` tags and `sitemap.xml` point at `https://mountandcraft.com/`. If you launch on a different domain, update those first.
- **External links:** Technic, CurseForge, Discord, Ko-fi and the live map open in a new tab and are not part of this repository.
- **Images:** the home page uses `.webp`; the guide screenshots are `.jpg`. Both are served with long-lived cache headers, so replace a file with a new name if you need to bust caches.