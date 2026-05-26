# end2end Clean Jekyll theme [![Deploy Pages](https://github.com/matheus-vieira/end2end-jekyll-theme/actions/workflows/pages.yml/badge.svg)](https://github.com/matheus-vieira/end2end-jekyll-theme/actions/workflows/pages.yml)



* [x] Clean layout
* [x] Resposive layout
* [x] Preprocessor SASS
* [x] CSS minified
* [x] Pagination
* [x] Syntax highlight
* [x] Author config
* [x] Comments with Disqus
* [ ] Search posts
* [ ] Share posts

---

## Configuration

Edit `_config.yml` before deploying:

- `url` — your site's base URL (e.g. `https://yourusername.github.io`)
- `baseurl` — subpath if not at root (e.g. `/end2end` or `""` for root)
- `author` — your personal info

### Analytics

- The theme ships a demo GA4 Measurement ID in `_config.yml` for the maintainer/demo site.
- To use your own analytics, replace the `google_analytics` value with your GA4 Measurement ID (format: `G-XXXXXXXXXX`).
- To disable analytics entirely, set `google_analytics` to an empty string in `_config.yml`.

## Current stack

- Ruby 3.2+
- Bundler 4.0+
- Jekyll 4.4 (Gemfile updated to allow 4.4.x)
- GitHub Pages via GitHub Actions
- Sass modules using `@use`

Note: The `Gemfile` was updated to accept Jekyll 4.4.x to match modern
local environments. If you update your system Jekyll, run:

```bash
bundle install
```

Always run Jekyll via Bundler in this repo:

```bash
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

## Local development

Install dependencies:

```bash
bundle install
```

Start local server:

```bash
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

Validate generated HTML and internal links:

```bash
rake check_links
```

## Pull request validation

Every pull request runs:

- Jekyll production build
- Ruby 3.2 validation
- Ruby 3.3 validation
- Ruby 3.4 validation

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

Repository workflow:

- `main` only accepts changes through pull requests.
- `epic/*` branches only accept changes through pull requests.
- Squash merge is the default merge strategy.
- Releases should be created from Git tags.

## Deploy

Deploy is fully handled by GitHub Actions.

Requirements:

- Repository Settings
- Pages
- Source = GitHub Actions

Merges into `main` trigger deployment automatically.
Direct pushes to `main` are blocked by repository rules.

## Migration notes

This project no longer uses:

- Travis CI
- `gh-pages` branch deployment
- Bourbon Sass framework

The site is now built and deployed using GitHub Actions artifacts.

---

### Using Rake tasks

* Show available tasks (default): `rake` or `rake help`
* Create a new page: `rake page name="contact.md"`
* Create a new post: `rake post title="TITLE OF THE POST"`
* Validate generated HTML and internal links: `rake check_links`

---

### Preview

![end2end Jekyll theme screenshot](/screenshot.png)

---

### Copyright and license

It is under [the MIT license](/LICENSE).

> :warning:
  Please remove metas `<meta name="robots" content="noindex">` and `<meta name="googlebot" content="noindex">` in `source/_layouts/default.html`

Enjoy :yum:

## Asset cache busting

- **Automatic build-time cache-busting:** The theme injects a build-time timestamp into critical asset URLs (fonts, CSS, JS) so updated binaries are fetched after each build. The timestamp format is controlled by the `asset_cache_bust_format` setting in `_config.yml`.
- **Where it's defined:** See `_config.yml` for the `asset_cache_bust_format` value and `source/css/fonts.css` for the canonical `@font-face` that includes the generated timestamp.

