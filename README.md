# Lauren Y.

Personal site on Jekyll + GitHub Pages. Two looks:

- **Front page, About, section pages** (`/`, `/about/`, `/side-builds/`, `/product-data/`): `layout: front`, modelled on andreasgursky.com.
- **The Corner Shelf** (`/shelf/…`): a reading blog by Helena L., migrated from helenalyndon-reads.tumblr.com, on Minima with a Tumblr-matching skin.

## Run locally

Use **Ruby 3.3**, the version GitHub Pages builds with. The macOS system Ruby (2.6) is too old. Ruby 3.4+/4.x also breaks: Jekyll 3's dependencies call `String#tainted?`, which newer Ruby removed.

```bash
brew install ruby@3.3
echo 'export PATH="/opt/homebrew/opt/ruby@3.3/bin:/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"' >> ~/.zshrc && exec zsh
```

Troubleshooting `bundle install` (seen on this Mac):
- If conda's `(base)` is active, run `conda deactivate` first. Conda's old `ld` shadows Apple's.
- If you get `tapi error: malformed file … MacOSX27.0.sdk`, the Command Line Tools are older than the newest SDK. Run `export SDKROOT=/Library/Developer/CommandLineTools/SDKs/MacOSX26.5.sdk` and retry, or reinstall the Command Line Tools.

Then:

```bash
bundle install
bundle exec jekyll serve
```

Open http://localhost:4000. Edits rebuild automatically, except `_config.yml` (restart the server).

## Publish to GitHub Pages

1. `git init && git add . && git commit -m "Initial site"`
2. Create a GitHub repo and push `main` to it.
   - Named `<username>.github.io`: the site serves at the domain root. Nothing else to change.
   - Any other name (e.g. `corner-shelf`): it serves at `https://<username>.github.io/corner-shelf/`. Add `baseurl: "/corner-shelf"` to `_config.yml`. All links use `relative_url`, so they'll follow.
3. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `(root)`**.
4. Wait a minute or two, then visit the URL shown there.

## Front page

- **Stills**: a random one shows on each visit (the only JavaScript on the site, in `index.html`). To add one, put the image in `assets/stills/` and add an entry to `_data/stills.yml` (file, title, credit, color). `color` is the link hover colour while that still is shown (default: the still's most vivid main colour; ask Claude to compute it, or pick your own). Without JS, the first entry shows.
- **Menu**: `front_menu` in `_config.yml`.
- **Icons**: SoundCloud / GitHub / Email links are `social` in `_config.yml`. Icon SVGs live in `_includes/icons/` (SoundCloud/GitHub from Simple Icons).
- **About**: edit `about.html` (intro text, then a small italic disclaimer); photo is `assets/about/lauren.jpg`.
- **Side Builds / Product & Data**: "Coming soon" pages in `side-builds/` and `product-data/`.
- Styles: `assets/front.scss` (Archivo Narrow from Google Fonts).

## Corner Shelf cover image

Put a square image at `assets/cover.jpg`. See `assets/README.md`.

## Add a new post

Corner Shelf posts: create `_posts/YYYY-MM-DD-short-slug.md` (it's published under `/shelf/…` automatically):

```markdown
---
layout: post
title: "Book Title by Author: a subtitle"
date: 2026-11-01
tags: [Author Name, some literature]
---

Body text. *Italics* with asterisks.

> Blockquotes start with >.
```

Put images in `assets/images/` and reference them as `![]({{ '/assets/images/file.jpg' | relative_url }})`.
To link a post from the booklist: `[Title]({% post_url 2026-11-01-short-slug %})`.

## What's customised (and where)

Minima is a gem dependency, not copied in. Only these files override it:

| File | Purpose |
|---|---|
| `_includes/head.html` | Minima's head + the Source Serif 4 font link (analytics removed) |
| `_includes/footer.html` | Empty: Tumblr theme has no footer |
| `_includes/header.html` | Cover + title + nav + hairline (nav links come from `nav_links` in `_config.yml`) |
| `_layouts/home.html` | Plain list: date, title, tags |
| `_layouts/post.html` | Shows date and `#tags` under the post, like Tumblr |
| `shelf/tags.html` | `/shelf/tags` page: every tag with its posts; each `#tag` on the site links to its section |
| `_sass/corner-shelf.scss` | Colours, fonts, blockquotes, layout |

## Defaults chosen during migration

- **Main Blog** link in the Corner Shelf nav goes to the front page (`/`).
- **Blockquotes match Tumblr**: upright text with a thin beige left border, not italic.
- **Colours** measured from the Tumblr theme: background `#f0eee6`, text `#141413`, muted `#6c6b66`.
- **Fonts**: Source Serif 4 from Google Fonts (loaded in `_includes/head.html`), falling back to Georgia. Tumblr uses Tiempos, which isn't free; Source Serif 4 is its next fallback.
- **Booklist**: the "Read" section and the "Target date" line are `###` subheadings, in the same order as on Tumblr.
- **Tags**: one `/shelf/tags` page (works on GitHub Pages with no plugins). Tags match by exact spelling, so `British literature` and `british literature` would be two tags; pick one style. Number-only tags like `2026` are fine.
- **URLs**: Corner Shelf lives under `/shelf/` (posts: `/shelf/2026/10/19/antony-and-cleopatra.html`, set by `defaults` in `_config.yml`). Old Tumblr URLs are not redirected.
- **JavaScript**: only the random still on the front page. Minima's mobile hamburger menu is replaced by a nav row that wraps.
