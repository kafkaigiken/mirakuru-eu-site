# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

The public website for **Mirakuru Europa OÜ** (an Estonian company in the LaLoka
Labs group), live at **https://mirakuru.eu**. It is a small **Jekyll** static
site, built and deployed by **GitHub Pages' native Jekyll build** from the `main`
branch. Remote: `git@github.com:kafkaigiken/mirakuru-eu-site.git`.

## Architecture

The whole site is a single page assembled from three regions:

- `_layouts/default.html` is the shell: it renders `{% include header.html %}`,
  then the page `{{ content }}` inside `<main class="site-content">`, then
  `{% include footer.html %}`.
- `index.md` is the only content page (front matter → `layout: default`). Its two
  `##` headings are the **Consulting** and **Products** sections.
- `_data/company.yml` is the single source of truth for the company's legal
  details (name, address, registration number, VAT number). `_includes/footer.html`
  renders them via `site.data.company.*`. **Change these values only in
  `_data/company.yml`** — never hard-code them in the footer.
- `assets/css/style.css` holds all styling, keyed to `.site-header` /
  `.site-content` / `.site-footer`. No web fonts or external assets (keeps the
  build self-contained and fast).

## Commands

```bash
bundle config set --local path 'vendor/bundle'   # one-time: install gems in-project,
                                                 # not system-wide (avoids PermissionError)
bundle install                                   # installs Jekyll + deps
bundle exec jekyll serve --livereload            # local preview at localhost:4000
```

The Gemfile uses plain `jekyll` (~> 4.3), not the heavier `github-pages` gem —
GitHub Pages builds the live site with its own gem environment and ignores this
Gemfile, so it only affects local preview. A few native gem extensions (ffi,
eventmachine, http_parser.rb) still need a build toolchain:
`sudo apt install ruby-dev build-essential`.

There are no tests. "Building" for production is not something you run — GitHub
Pages does it on push.

## Deployment / releases

A release is just pushing to `main`; GitHub Pages rebuilds and deploys
automatically (watch `pages-build-deployment` under the Actions tab). See
`README.md` for the full release checklist. Two hard constraints:

- **Keep `CNAME`** (`mirakuru.eu`) at the repository root — removing it breaks the
  custom domain.
- **Stay plugin-free / within the GitHub Pages allowlist.** The site relies on the
  native Pages build (no CI workflow). Adding a non-allowlisted Jekyll plugin
  would silently fail to build on GitHub even if it works locally. Note the local
  `Gemfile` uses plain `jekyll` while GitHub builds with its own `github-pages`
  environment; keep the site to core Jekyll features (as it is now) so the two
  never diverge.

## Conventions & constraints

- **This repo is public — never commit anything genuinely private.** The company
  registration number and VAT number in `_data/company.yml` are public,
  legally-required company data and are intentionally included; API keys, personal
  data, and secrets are not.
- Company copy must be grounded in verifiable public information (e.g. the
  puchiden.app site, the LaLoka Labs group privacy policy). Do not invent claims,
  contact details, or legal identifiers.
- The page keeps a single visually-hidden `<h1>` (the company name) for
  accessibility/SEO, with the visible section headings as `<h2>`. Preserve one
  `<h1>` per page if the structure changes.
