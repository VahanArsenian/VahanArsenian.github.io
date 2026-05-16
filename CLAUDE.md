# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic site for Vahan Arsenyan, built with **Hugo extended** (>=0.128) using the **Hugo Blox Academic CV** theme. The theme is pulled in as a **Hugo Module** (Go modules), not vendored — see `hugo.yaml` `module.imports` and `go.mod`.

## Common commands

```bash
# Dev server with drafts (preferred for local edits)
hugo server -D
# or, with fast-render disabled (matches package.json `dev` script):
npm run dev

# Refresh / pin theme module versions
hugo mod get -u ./...

# Production-equivalent build (writes ./public)
hugo --gc --minify

# Full build + Pagefind static search index (matches package.json `build`)
npm run build
```

CI does NOT run `npm run build`; the Actions workflow runs `hugo --gc --minify --baseURL ...` directly, so the Pagefind index from `npm run build` is a local-only artifact. The `search.provider: wowchemy` setting in `config/_default/params.yaml` is what the theme actually uses in production.

There are no tests and no linter configured.

## Architecture

### Content + theme overlay
- `hugo.yaml` is the root config; per-section settings live under `config/_default/` (`params.yaml`, `menus.yaml`, `languages.yaml`, `module.yaml`).
- All visible content is Markdown with YAML front matter under `content/`. The theme renders sections via "widget blocks" listed in `content/_index.md` under `sections:` — the order there is the order they appear on the home page.
- `layouts/` **overrides** theme templates of the same path. Editing a file under `layouts/` shadows the theme's version; the theme code itself lives in the Hugo Module cache, not in this repo.

### Custom hobbies feature (not part of the theme)
The Games/Anime browser is a bespoke addition layered on top of the academic-cv theme:

- Front matter `type: games` or `type: anime` (cascaded from `content/hobbies/{games,anime}/_index.md`) routes pages to `layouts/games/` and `layouts/anime/` templates.
- `/hobbies/` is a hub page rendered by `layouts/hobbies/list.html`, which calls `partials/hobby-hub-tile.html` per medium.
- The grid + filter UI is shared via `partials/hobby-card-grid.html`, `partials/hobby-card.html`, `partials/hobby-filter-script.html`, `partials/hobby-styles.html`. Both games and anime go through these — when changing card rendering, edit the partials, not the per-medium templates.
- Card filtering keys (`data-status`, `data-rating`, `data-year`, `data-genres`) are produced in `partials/hobby-card.html` and consumed client-side by `partials/hobby-filter-script.html`. Adding a new filter dimension means changing both files plus the calling list template's `statusAll` (or analogous) slice.
- Cover images live under `assets/media/` (NOT `static/`). Front matter `cover:` is a path relative to `assets/media/`, resolved via `resources.Get` so Hugo's asset pipeline processes them. Page-bundle resources are a fallback.

### Deployment
- `.github/workflows/deploy.yml` builds on every push to `main` and deploys to GitHub Pages.
- The workflow auto-injects `--baseURL` from the Pages runtime, so the `baseURL` in `hugo.yaml` only matters for local previews / RSS canonicalization.
- One-time setup: GitHub repo → Settings → Pages → Source: **GitHub Actions**.

## Editing conventions

- Theme widgets (publications, talks, teaching, interests, education) come from the theme — to change their look, override the theme partial under `layouts/_partials/` rather than forking the whole template.
- Publications, talks, teaching: each entry is a folder under `content/<section>/<slug>/index.md` (page bundle, so co-located images/`cite.bib` are picked up automatically).
- Hobbies entries: a single `.md` file under `content/hobbies/{games,anime}/<slug>.md` — no folder bundle. Required front matter shape is documented in `README.md`; the cards will silently omit fields that are missing.
