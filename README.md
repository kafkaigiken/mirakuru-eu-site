# Mirakuru Europa OÜ Public Site

The public website for **Mirakuru Europa OÜ**, live at **https://mirakuru.eu**.

It is a small [Jekyll](https://jekyllrb.com/) static site, built and served
automatically by **GitHub Pages** from the `main` branch. The site is three
regions — a header, the page content, and a footer with the company's legal
details.

## Project layout

| Path | What it holds |
| --- | --- |
| `index.md` | The homepage content (the **Consulting** and **Products** sections). |
| `_data/company.yml` | Company legal details (name, address, registration number, VAT number) shown in the footer. **Edit these here — nowhere else.** |
| `_includes/header.html` | The header / wordmark (swap for a logo later). |
| `_includes/footer.html` | The footer, which renders `_data/company.yml`. |
| `_layouts/default.html` | The page shell that stitches header + content + footer together. |
| `assets/css/style.css` | All styling. |
| `_config.yml` | Site-wide settings. |
| `CNAME` | Binds the `mirakuru.eu` custom domain. **Do not delete.** |

## Local development

Requires Ruby (3.x) plus its build toolchain (Jekyll's dependencies include
native C extensions) and [Bundler](https://bundler.io/). On Debian/Ubuntu:

```bash
sudo apt install ruby ruby-dev build-essential ruby-bundler
```

Install this site's gems **into the project**, not the system-wide gem directory.
This is important: by default Bundler tries to write to `/var/lib/gems`, which is
root-owned, and fails with `Bundler::PermissionError`. Do **not** work around that
with `sudo` — instead point Bundler at a project-local path (one-time; it writes
`.bundle/config`, and `vendor/` is already git-ignored):

```bash
bundle config set --local path 'vendor/bundle'
bundle install
```

Then run the live-reloading preview at http://localhost:4000:

```bash
bundle exec jekyll serve --livereload
```

The preview rebuilds automatically as you edit.

> The `Gemfile` pins plain **Jekyll** for local preview only. GitHub Pages builds
> the published site with its own toolchain and ignores this `Gemfile`, so the two
> stay independent — keep to core Jekyll features and they render the same.

## Making a release (publishing changes)

GitHub Pages rebuilds and deploys the site itself — there is **no separate build
step to run and nothing to upload**. A release is simply merging your changes to
`main`.

1. **Preview locally** with `bundle exec jekyll serve` and confirm the change looks right.
2. **Commit** your changes:
   ```bash
   git add -A
   git commit -m "Describe the change"
   ```
3. **Push to `main`** (directly, or by merging a pull request):
   ```bash
   git push origin main
   ```
4. **GitHub Pages builds and deploys automatically.** Watch the
   `pages-build-deployment` run under the repo's **Actions** tab; it usually
   finishes within a minute or two.
5. **Verify** the change at https://mirakuru.eu (hard-refresh to bypass cache).

### Optional: tag a release

Pushing to `main` is what actually publishes the site. If you also want a
named marker for a milestone, add a git tag:

```bash
git tag -a v1.1 -m "Add pricing section"
git push origin v1.1
```

Tags are for your own history and changelog; they do **not** trigger or change
the deploy.

### One-time GitHub Pages setup

If Pages was never configured for this repo: go to **Settings → Pages**, set
**Source** to *Deploy from a branch*, choose branch **`main`** and folder
**`/ (root)`**, and ensure the **Custom domain** field reads `mirakuru.eu` (this
is what the `CNAME` file provides).

## Editing common things

- **Company address / registration / VAT number:** edit `_data/company.yml`.
- **Page text (Consulting, Products):** edit `index.md`.
- **Look and feel:** edit `assets/css/style.css`.
- **Replace the wordmark with a logo:** see the comment in `_includes/header.html`.
