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

## Theme UI features

- **Tags** — add `tags = ["hugo", "meta"]` to a post's front matter. They render as clickable pills and auto-list at `/tags/<tag>/` via Hugo's standard taxonomy system.
- **Table of contents** — automatic, no config needed. Built client-side from the post's headings (`toc.js`); opened via the list icon at the top-left of each post.
- **Dark/light mode toggle** — the switch icon in the top nav bar (`theme-mode.js`). Respects OS preference by default; readers can override.
- **Pinning a post** — add `pin = true` to front matter to keep it pinned to the top of the post list.
- **Search** — set `enableSearch = true` under `[params]` in `hugo.toml`. Indexes content client-side (no server needed) and adds a search box to the nav.
- **Comments (Gitalk)** — set `enableGitalk = true` plus a `[params.gitalk]` block (GitHub OAuth app client ID/secret, repo, owner). Comments are stored as GitHub issues on a repo you choose.
- **RSS** — already on (`rss = true` in `hugo.toml`); feed is auto-generated at `/index.xml`.
- **Shortcodes** — this theme adds `{{</* details "Click to expand" */>}}...{{</* /details */>}}` for collapsible sections. Hugo's built-ins also work, e.g. `{{</* figure src="..." caption="..." */>}}`, `{{</* youtube id */>}}`. Code fences get syntax highlighting automatically via the Pygments config already in `hugo.toml`.

## Publishing

Update `baseURL` in `hugo.toml` to your real domain before deploying, then run `hugo` and serve the contents of `public/`.

## Deployment (CI)

`.gitea/workflows/deploy.yml` runs on every push to `main`: it checks out the repo (with submodules, for the theme), builds the site with Hugo, and deploys `public/` as static assets to a Cloudflare Worker (`wrangler.toml`, name `blog`) via `wrangler deploy`. `wrangler deploy` creates the Worker automatically on first run — no manual dashboard step needed.

This requires two repo secrets set in Gitea (Settings → Actions → Secrets):
- `CLOUDFLARE_API_TOKEN` — must have the **Workers Scripts:Edit** permission (use the "Edit Cloudflare Workers" token template in the Cloudflare dashboard)
- `CLOUDFLARE_ACCOUNT_ID`
