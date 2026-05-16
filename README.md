# vahanarsenyan.com (source)

Personal / academic site for **Vahan Arsenyan** — PhD candidate at CREST,
ENSAE, Institut Polytechnique de Paris.

Built with [Hugo](https://gohugo.io) (extended) + the
[Hugo Blox Academic CV](https://github.com/HugoBlox/theme-academic-cv) theme,
pulled in as a Hugo Module. Deployed to GitHub Pages via GitHub Actions.

## Local development

Prerequisites:

- Hugo **extended** >= 0.128
- Go >= 1.22 (for Hugo Modules)
- Node.js >= 18 (only if the theme ships JS tooling you want to hack on)

Install, pull the theme, and run a live-reloading server:

```bash
hugo mod get -u ./...
hugo server -D
```

Open <http://localhost:1313>. Drafts (`draft: true`) are shown with `-D`.

To reproduce the production build locally:

```bash
hugo --gc --minify
```

The rendered site is written to `./public`.

## Repository layout

```
hugo.yaml                        # top-level site config (title, baseURL, modules)
config/_default/                 # params, menus, languages
content/
  _index.md                      # home page: section order
  authors/admin/_index.md        # profile (bio, interests, links, skills)
  experience/                    # work experience entries
  publication/                   # papers (one dir per paper)
  talk/                          # conferences & invited talks
  teaching/                      # courses
  post/                          # optional blog
  hobbies/
    _index.md                    # hub page: Games / Anime tiles
    games/                       # one Markdown file per game
    anime/                       # one Markdown file per anime
layouts/
  partials/                      # shared hobby-card-grid + hobby-card
  hobbies/                       # hub list template
  games/                         # list + single templates for /hobbies/games/
  anime/                         # list + single templates for /hobbies/anime/
assets/media/                    # images (profile photo, covers)
.github/workflows/deploy.yml     # build + deploy to GitHub Pages
go.mod                           # Hugo Modules manifest (theme pinned here)
```

## Editing content

All content is Markdown with YAML front matter. Changes pushed to `main` are
deployed automatically.

- **Profile, bio, interests, skills, social links**: edit
  `content/authors/admin/_index.md`.
- **Home page section order**: edit `content/_index.md` (each block toggles a
  theme widget).
- **Experience / Education**: edit the relevant file under
  `content/experience/` or the `education` block in `authors/admin/_index.md`.
- **Publication**: create a folder `content/publication/<slug>/` with an
  `index.md` and (optionally) `cite.bib`. Use existing entries as a template.
- **Talk**: create `content/talk/<slug>/index.md`. The theme renders dates,
  venue, links, and slides.
- **Teaching**: create `content/teaching/<slug>/index.md`.
- **Hobbies - add a game**: create
  `content/hobbies/games/<slug>.md` with front matter like:

  ```yaml
  ---
  title: "Elden Ring"
  cover: "covers/elden-ring.jpg"
  rating: 9.5
  status: "played"          # played | playing | backlog | dropped
  platform: ["PC"]
  genre: ["Action RPG", "Soulslike"]
  hours: 120
  year_completed: 2024
  summary: "One-liner."
  ---
  Longer notes / mini-review in Markdown.
  ```

- **Hobbies - add an anime**: create
  `content/hobbies/anime/<slug>.md` with:

  ```yaml
  ---
  title: "Frieren: Beyond Journey's End"
  cover: "covers/frieren.jpg"
  rating: 9.8
  status: "watched"         # watched | watching | plan-to-watch | dropped
  studio: "Madhouse"
  genre: ["Fantasy", "Slice of Life"]
  episodes: 28
  year_completed: 2024
  summary: "One-liner."
  ---
  Longer notes / mini-review in Markdown.
  ```

- **Covers / images**: drop files under `assets/media/covers/` and reference
  them by path relative to `assets/media/` in the front matter (`cover:`).

## Deployment

On every push to `main`, `.github/workflows/deploy.yml`:

1. installs Hugo extended + Dart Sass + Go,
2. pulls Hugo modules (theme code) via `hugo mod get -u ./...`,
3. builds the site with `hugo --gc --minify --baseURL <pages-url>/`,
4. uploads `./public` as a Pages artifact,
5. deploys to the `github-pages` environment.

You can also trigger a deploy manually from the Actions tab via
**Run workflow** (the workflow exposes `workflow_dispatch`).

### Repo name and baseURL

- If the repo is named `vahanarsenyan.github.io`, the site serves at the apex
  URL (`https://vahanarsenyan.github.io/`).
- Otherwise it serves at `/<repo>/`. The workflow auto-sets `--baseURL` from
  the Pages runtime config, so this works for either case without edits.

The value in `hugo.yaml` (`baseURL: https://vahanarsenyan.github.io/`) is used
for local previews and RSS/sitemap canonicalization; the workflow overrides it
at build time.

### Custom domain

Drop a `CNAME` file at the repo root with the domain on a single line
(e.g. `vahanarsenyan.com`) and configure DNS per
[GitHub's docs](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site).
Also update `baseURL` in `hugo.yaml` to match.

### Enabling Pages (one-time setup)

In the repo on GitHub: **Settings -> Pages -> Build and deployment ->
Source: GitHub Actions**. The workflow will take over from there.
