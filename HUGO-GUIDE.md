# Hugo Basics

Quick reference for working with this site. Full docs: https://gohugo.io/documentation/

## Adding a new page/post

```
hugo new content post/my-new-post.md
```

This creates `content/post/my-new-post.md` using the archetype in `archetypes/default.md`. Note it's `post` (singular) — that's the path the github-style theme expects. It's created with `draft = true` in the front matter — set it to `false` (or build with `-D`) before it will appear in the published site.

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

- `content/` — your Markdown pages and posts (the actual site content); posts go in `content/post/`
- `themes/github-style/` — the theme (git submodule), controls layout/styling
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

## Theme: github-style

This site uses [github-style](https://github.com/MeiK2333/github-style), a theme styled after GitHub's README rendering. A few things specific to it:

- Posts must live under `content/post/` (singular), not `content/posts/`.
- Site-wide theme options (author, description, avatar, social links, RSS, comments via Gitalk, Google Analytics, etc.) are set under `[params]` in `hugo.toml` — see the theme's [README](https://github.com/MeiK2333/github-style#readme) for the full list.
- To limit how much of a post shows on the listing page, either set `summary = "..."` in front matter, or add a `<!--more-->` marker in the post body to split preview from full content.

## Publishing

Update `baseURL` in `hugo.toml` to your real domain before deploying, then run `hugo` and serve the contents of `public/`.

## Deployment (CI)

`.gitea/workflows/deploy.yml` runs on every push to `main`: it checks out the repo (with submodules, for the theme), builds the site with Hugo, and deploys `public/` to Cloudflare Pages (project `blog`) via `wrangler`.

This requires two repo secrets set in Gitea (Settings → Actions → Secrets):
- `CLOUDFLARE_API_TOKEN`
- `CLOUDFLARE_ACCOUNT_ID`

The Cloudflare Pages project doesn't need to be created manually — `wrangler pages deploy` creates it automatically on first run if a project named `blog` doesn't already exist.
