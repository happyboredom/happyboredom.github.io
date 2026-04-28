# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal Jekyll blog (jekyll-now theme) hosted on GitHub Pages at https://happyboredom.github.io. Deployment is automatic — pushing to `master` triggers a GitHub Pages rebuild with no CI step required.

The site has two content types: **posts** (standard blog entries) and **editions** (curated link roundups with a dedicated layout and data model).

## Local Development

Install dependencies (mirrors GitHub Pages plugins):
```bash
gem install github-pages
```

Serve locally with live reload:
```bash
jekyll serve
```

View at http://127.0.0.1:4000/. There is no `Gemfile` committed — `Gemfile` and `Gemfile.lock` are gitignored.

## Creating Posts

Posts go in `_posts/` with the filename format `YYYY-MM-DD-title.md`. Required front matter:

```yaml
---
layout: post
title: Your Post Title
---
```

URLs are generated as `/:title/` (no date in the URL, per `permalink: /:title/` in `_config.yml`).

## Editions

Editions are curated link roundups (like a newsletter issue). They are a Jekyll collection (`_editions/`) with their own layout and data files.

### Creating an edition

1. Add a document to `_editions/` named `YYYY-MM-DD-<slug>.md`:

```yaml
---
layout: edition
title: "Food"
date: 2019-02-13
items:
  - swedish-crispbreads
  - vegan-egg
---
```

The `items` list controls which articles appear and in what order.

2. Add one YAML file per item to `_data/editions/<slug>/` — where `<slug>` matches the part of the filename after the date (e.g. `_data/editions/food/` for `2019-02-13-food.md`):

```yaml
# _data/editions/food/some-article.yml
layout: article   # or: hero, image
title: "Article Title"
url: "https://example.com/article"
domain: "example.com"
excerpt: "A short description."
image: "https://example.com/image.jpg"
```

### Item layouts

Each item declares a `layout` field that maps to an include in `_includes/items/`:

| `layout` value | Include | Appearance |
|---|---|---|
| `hero` | `_includes/items/hero.html` | Full-width image above body text |
| `article` | `_includes/items/article.html` | Side-by-side image + body text |
| `image` | `_includes/items/image.html` | Image-only with caption overlay |

All item types share the same fields: `title`, `url`, `domain`, `excerpt`, `image`.

### How the data lookup works

`_layouts/edition.html` iterates `page.items` and resolves each key via:

```liquid
site.data.editions[page.slug][key]
```

`page.slug` is derived by Jekyll from the filename (date prefix stripped), so `2019-02-13-food.md` → slug `food` → reads from `_data/editions/food/`.

### Editions index

`editions/index.html` lists all editions, sorted newest-first, using the `site.editions` collection. URLs are `/editions/<name>/` (configured via `permalink: /editions/:name/` in `_config.yml`).

## Architecture

**Layout chains:**
- Blog posts: `post.html` → `default.html`
- Pages: `page.html` → `default.html`
- Editions: `edition.html` → `default.html`

**`default.html`** is the root template. It pulls in three includes:
- `meta.html` — `<head>` meta/OG tags; uses `page.excerpt` when available, falls back to `site.description`
- `svg-icons.html` — footer social icons; which icons appear is controlled by the `footer-links` map in `_config.yml`
- `analytics.html` — Google Analytics snippet, only active when `google_analytics` is set in `_config.yml`

**Styling:** `style.scss` is the entry point (requires the empty `---` front matter so Jekyll processes it). It `@import`s partials from `_sass/`: `_reset`, `_variables`, `_highlights`, `_svg-icons`. The mobile breakpoint mixin is defined in `_variables.scss` at 640px.

**Optional integrations** (configured in `_config.yml`, currently disabled):
- Disqus comments — set `disqus:` to your shortname
- Google Analytics — set `google_analytics:` to your tracking code

**Plugins in use:** `jekyll-sitemap`, `jekyll-feed` (generates `/sitemap.xml` and `/feed.xml` automatically). Both are on the [GitHub Pages whitelist](https://pages.github.com/versions/). Only whitelisted plugins will run on GitHub Pages — adding an unlisted gem in `plugins:` in `_config.yml` will be silently ignored remotely (it may work locally but won't on the live site).
