# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal Jekyll blog (jekyll-now theme) hosted on GitHub Pages at happyboredom.github.io. The site is rebuilt automatically by GitHub Pages on every push to `master`.

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

## Architecture

**Layout chain:** `post.html` → `default.html`, `page.html` → `default.html`

**`default.html`** is the root template. It pulls in three includes:
- `meta.html` — `<head>` meta/OG tags; uses `page.excerpt` when available, falls back to `site.description`
- `svg-icons.html` — footer social icons; which icons appear is controlled by the `footer-links` map in `_config.yml`
- `analytics.html` — Google Analytics snippet, only active when `google_analytics` is set in `_config.yml`

**Styling:** `style.scss` is the entry point (requires the empty `---` front matter so Jekyll processes it). It `@import`s partials from `_sass/`: `_reset`, `_variables`, `_highlights`, `_svg-icons`. The mobile breakpoint mixin is defined in `_variables.scss` at 640px.

**Optional integrations** (configured in `_config.yml`, currently disabled):
- Disqus comments — set `disqus:` to your shortname
- Google Analytics — set `google_analytics:` to your tracking code

**Plugins in use:** `jekyll-sitemap`, `jekyll-feed` (generates `/sitemap.xml` and `/feed.xml` automatically).
