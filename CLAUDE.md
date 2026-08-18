# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal blog built with [Hugo](https://gohugo.io/) (static site generator), using the [github-style](https://github.com/MeiK2333/github-style) theme as a git submodule. There is no custom application code — content is Markdown, presentation comes from the theme, and Hugo compiles the two into static HTML.

## Commands

```
hugo server -D      # run local dev server with live reload at http://localhost:1313/, including drafts
hugo server         # same, but only published (non-draft) content
hugo                # build the static site into public/ (build artifact, gitignored)
hugo new content post/<name>.md    # create a new post from the archetype in archetypes/default.md (note: singular "post", required by this theme)
hugo new content <name>.md         # create a standalone page (e.g. about.md)
```

Cloning this repo elsewhere requires the theme submodule:
```
git clone --recurse-submodules <repo-url>
# or, on an existing clone:
git submodule update --init --recursive
```

## Architecture

- `content/` — all site content as Markdown files with TOML front matter (`+++ ... +++`). Posts live under `content/post/` (singular — required by this theme); the URL structure mirrors this directory structure.
- `themes/github-style/` — the theme, vendored as a git submodule. Do not edit files inside it directly — changes belong upstream or as overrides (see below). Theme-wide options (author, avatar, social links, RSS, Gitalk comments, analytics) are configured under `[params]` in `hugo.toml`.
- `layouts/` — project-level template overrides. A file here at the same relative path as a theme template takes precedence over the theme's version, letting you customize rendering without forking the theme.
- `archetypes/default.md` — the template used when scaffolding new content via `hugo new content`.
- `static/` — files copied verbatim into the site root at build time (images, favicon, etc.).
- `hugo.toml` — site configuration: `baseURL`, `title`, `theme`, and any theme params/menus.
- `public/` — generated output of `hugo` builds; gitignored, never edit or commit.

New posts are created with `draft = true` by default (from the archetype); set `draft = false` in front matter before they'll appear in a non-`-D` build/server.

See `HUGO-GUIDE.md` for a fuller walkthrough of adding pages and Hugo basics.
