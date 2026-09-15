# chowdown (recipes.solace.org)

Jekyll site (Tailwind-CDN era, no build step) — a real GitHub fork of
`clarklab/chowdown`, not a from-scratch project.

## Remotes & deploy

- `origin` = your fork (`edinel/chowdown`), `chowdown` = upstream
  (`clarklab/chowdown`, default branch `gh-pages`).
- Live branch is `main`; GitHub Pages deploys from `main` on push.
- Pull upstream: `git fetch chowdown && git merge chowdown/gh-pages`. Never
  use GitHub's "Sync fork" button — it conflicts with local branding.
- Feature branches meant for upstream: branch off `chowdown/gh-pages`, PR to
  `clarklab:gh-pages`, then separately merge to `main` for the live site.

## No local build

No `Gemfile` in this repo — GitHub Pages builds it server-side. Verify
changes by pushing a branch and checking the Pages build status, or by
checking the live deploy after merging to `main`:
`gh api repos/edinel/chowdown/pages/builds/latest --jq .status` → `built`.
`docker-compose.yml` (inherited from upstream) can run a local `jekyll serve`
preview via `docker compose up` if needed.

## Recipe front matter conventions

- `_recipes/*.md`, filename is the recipe title in CamelCase, no spaces.
- `image: default.jpg` when there's no real photo yet (14+ recipes do this;
  file already exists at `images/default.jpg`).
- Section headers: an `ingredients` or `directions` entry starting with `# `
  (e.g. `"# For the sofrito"`) renders as a styled heading instead of a list
  item — no checkbox, no schema.org `itemprop`. Documented in `readme.md`.
- Tags are lowercase-hyphenated YAML lists.

## Liquid gotcha

Filters are not allowed inside `{% if %}` tags — `{% if x | slice: 0,2 ==
"# " %}` silently fails to parse as intended. Assign first:
`{% assign p = x | slice: 0, 2 %}{% if p == "# " %}`.

## chit

This repo tracks agent working-state with `chit` (durable, git-backed).
Run `chit ls` at the start of a session to see open work before starting new work.
Record status and handoffs with `chit set` / `chit note`; never write secrets into a ledger entry.

docs: run `chit quickstart` for agent doctrine; admin runbook (mirror/force-push hazards, secrets incidents) at ledger/docs/admin.md
