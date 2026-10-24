# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal blog built with [Hugo](https://gohugo.io/) (static site generator), using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme as a git submodule. There is no custom application code — content is Markdown, presentation comes from the theme, and Hugo compiles the two into static HTML.

## Commands

```
hugo server -D      # run local dev server with live reload at http://localhost:1313/, including drafts
hugo server         # same, but only published (non-draft) content
hugo                # build the static site into public/ (build artifact, gitignored)
hugo new content post/<name>.md    # create a new post from the archetype in archetypes/default.md (note: singular "post", matching `mainSections = ['post']`)
hugo new content <name>.md         # create a standalone page (e.g. about.md)
```

Cloning this repo elsewhere requires the theme submodule:
```
git clone --recurse-submodules <repo-url>
# or, on an existing clone:
git submodule update --init --recursive
```

## Architecture

- `content/` — all site content as Markdown files with TOML front matter (`+++ ... +++`). Posts live under `content/post/` (singular — `mainSections = ['post']` in `hugo.toml` lists this section on the home page); the URL structure mirrors this directory structure.
- `themes/PaperMod/` — the theme, vendored as a git submodule. Do not edit files inside it directly — changes belong upstream or as overrides (see below). Theme options (home info, social icons, footer, `mainSections`, assets) are configured under `[params]` in `hugo.toml`. PaperMod shortcodes live in `themes/PaperMod/layouts/_shortcodes/` (e.g. `video`).
- `layouts/` — project-level template overrides. A file here at the same relative path as a theme template takes precedence over the theme's version, letting you customize rendering without forking the theme.
- `archetypes/default.md` — the template used when scaffolding new content via `hugo new content`.
- `static/` — files copied verbatim into the site root at build time (images, favicon, etc.).
- `hugo.toml` — site configuration: `baseURL`, `title`, `theme`, and any theme params/menus.
- `public/` — generated output of `hugo` builds; gitignored, never edit or commit.

New posts are created with `draft = true` by default (from the archetype); set `draft = false` in front matter before they'll appear in a non-`-D` build/server.

See `HUGO-GUIDE.md` for a fuller walkthrough of adding pages and Hugo basics.

## Deployment

`.gitea/workflows/deploy.yml` builds and deploys on every push to `main`: checkout (with submodules) → `hugo --minify` → `wrangler deploy`, which pushes `public/` (see `wrangler.toml`'s `[assets]` directory) as static assets to a Cloudflare Worker named `blog`. Requires `CLOUDFLARE_API_TOKEN` (needs Workers Scripts:Edit permission) and `CLOUDFLARE_ACCOUNT_ID` as Gitea repo secrets; the Worker is created automatically on first deploy.
