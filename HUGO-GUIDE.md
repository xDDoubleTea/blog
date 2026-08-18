# Hugo Basics

Quick reference for working with this site. Full docs: https://gohugo.io/documentation/

## Adding a new page/post

```
hugo new content posts/my-new-post.md
```

This creates `content/posts/my-new-post.md` using the archetype in `archetypes/default.md`. It's created with `draft = true` in the front matter — set it to `false` (or build with `-D`) before it will appear in the published site.

Front matter (the `+++ ... +++` block at the top of the file) holds metadata:

```toml
+++
date = '2026-08-18T11:36:52+08:00'
draft = false
title = 'My New Post'
+++

Your Markdown content goes here.
```

To create a page outside of `posts/` (e.g. an "About" page), give it a different path:

```
hugo new content about.md
```

## Previewing the site locally

```
hugo server -D
```

`-D` includes draft content. The server watches for file changes and live-reloads at http://localhost:1313/. Omit `-D` to preview exactly what would be published.

## Building the site

```
hugo
```

Generates the static site into `public/`. That's what you'd upload to a web server. (`public/` is gitignored — it's a build artifact, not source.)

## Project layout

- `content/` — your Markdown pages and posts (the actual site content)
- `themes/ananke/` — the theme (git submodule), controls layout/styling
- `layouts/` — put files here to override individual theme templates
- `static/` — files copied as-is into the built site (images, favicon, etc.)
- `archetypes/` — templates used when you run `hugo new content ...`
- `hugo.toml` — site configuration (title, baseURL, theme, menus, params, etc.)

## Cloning this repo elsewhere

Because the theme is a git submodule, clone with:

```
git clone --recurse-submodules <repo-url>
```

or, if already cloned:

```
git submodule update --init --recursive
```

## Publishing

Update `baseURL` in `hugo.toml` to your real domain before deploying, then run `hugo` and serve the contents of `public/`.
