# imrenhe.github.io

Personal academic website for He Ren, built with [Quarto](https://quarto.org/) and
deployed to GitHub Pages at <https://imrenhe.github.io/>.

## Structure

```
.
├── _quarto.yml                # Site configuration (navbar, theme, footer, favicon)
├── index.qmd                  # Home page (about-trestles bio + links)
├── about.qmd                  # About page (bio, education, certifications)
├── teaching.qmd               # Teaching (courses by institution)
├── research.qmd               # Research (interests, journal articles, working papers)
├── photography.qmd            # Photo gallery (layout-ncol grid)
├── 404.qmd                    # Custom 404 page (rendered to 404.html for GitHub Pages)
├── blog/
│   ├── index.qmd              # Blog listing page (auto-generated from posts/)
│   └── posts/<slug>/index.qmd # One folder per post
├── images/
│   ├── stock-2.svg            # Favicon and navbar logo
│   ├── profile.jpeg           # Profile photo on the home page
│   └── photo/                 # Photos shown on the photography page
├── styles/
│   ├── styles.css             # Custom CSS overrides (e.g. SDR limit on profile photo)
│   ├── light.scss             # Light-theme SCSS (colors, fonts)
│   └── dark.scss              # Dark-theme SCSS (colors, fonts)
└── _site/                     # Build output (gitignored; pushed to gh-pages by quarto publish)
```

## Prerequisites

- [Quarto](https://quarto.org/docs/get-started/) (this site was set up with 1.9.37).
- [Git](https://git-scm.com/).

## Local preview

Run a local development server with live reload:

```sh
quarto preview
```

This watches the source files and rebuilds on save.

> **Note on caching:** Quarto keeps a `.quarto/` cache. After larger structural
> changes (e.g. switching a page's frontmatter from a `listing:` page to a plain
> page), stale artifacts can survive into the next render. If a page renders
> incorrectly after such a change, stop preview, run `rm -rf .quarto _site`, then
> re-render or re-preview.

## Build

To produce a static build in `_site/`:

```sh
quarto render
```

`_site/` is local-only — it is gitignored and gets pushed to the `gh-pages` branch by
`quarto publish` (see below).

## Deployment to GitHub Pages

This site uses Quarto's `gh-pages` workflow. Render output is pushed to a separate
`gh-pages` branch, and GitHub Pages serves the site from there.

### One-time setup

1. Push this repository to GitHub at `imrenhe/imrenhe.github.io`.
2. In the GitHub repo settings → **Pages**, set:
   - **Source:** Deploy from a branch
   - **Branch:** `gh-pages` / `(root)`
3. (Quarto will create the `gh-pages` branch on first publish — you don't need to
   create it manually.)

### Publishing

From the project root, with a clean working tree:

```sh
quarto publish gh-pages
```

This will:

1. Render the site.
2. Commit the rendered output to the `gh-pages` branch.
3. Push that branch to `origin`.

The first publish prompts for confirmation; subsequent publishes go through
without prompts. The site goes live at <https://imrenhe.github.io/> within a
minute or two.

## Adding content

### Blog post

Create `blog/posts/<slug>/index.qmd` with frontmatter:

```yaml
---
title: "Post title"
description: "One-line summary that appears in the listing."
author: "He Ren"
date: "YYYY-MM-DD"
categories: [tag1, tag2]
---
```

The blog listing at `/blog` picks it up automatically.

### Photo

Add the image file to `images/photo/`, then add a figure to [photography.qmd](photography.qmd)
inside the `::: {layout-ncol=3}` block:

```markdown
![Caption](images/photo/your-photo.jpg)
```

### Profile photo

Replace `images/profile.jpeg` (referenced by [index.qmd](index.qmd)).

### Favicon

Replace [images/stock-2.svg](images/stock-2.svg), or point `favicon:` and
`navbar.logo:` in [_quarto.yml](_quarto.yml) at a different SVG.

## Theme & styling

- **Theme:** [`cosmo`](https://bootswatch.com/cosmo/) Bootswatch theme via Quarto.
  Change in `_quarto.yml` under `format.html.theme`.
- **Custom CSS:** [styles/styles.css](styles/styles.css). Currently it limits the profile photo to
  SDR (the source JPEG carries an HDR gain map) via `dynamic-range-limit: standard`
  on `.about-image`.
