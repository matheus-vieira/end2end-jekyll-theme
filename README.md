# end2end Clean Jekyll theme [![Deploy Pages](https://github.com/matheus-vieira/end2end-jekyll-theme/actions/workflows/pages.yml/badge.svg)](https://github.com/matheus-vieira/end2end-jekyll-theme/actions/workflows/pages.yml)



* [x] Clean layout
* [x] Responsive layout
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

### Environment Configuration (GitHub Secrets)

The theme build process is extensible through **Repository Secrets**, allowing you to inject sensitive or environment-specific configurations without committing them to the code.

#### How it works

1. Define secrets in **Settings → Secrets and variables → Actions → Repository secrets**.
2. The GitHub Actions workflow maps those values to environment variables and runs `bundle exec rake config:secrets`.
3. The rake task generates a temporary `_config.secrets.yml` file (ignored by git), validates it, and passes it to Jekyll:
  `bundle exec jekyll build --config _config.yml,_config.assets.yml,_config.secrets.yml`.

#### Available Secrets

| Secret | Target Jekyll Key | Requirement | Description |
| :--- | :--- | :--- | :--- |
| `GA_MEASUREMENT_ID` | `ga_measurement_id` | Optional | Google Analytics 4 ID (for example, `G-XXXX`). Invalid values are skipped with a warning. |
| `GA_OPTIONS` | `ga_options` | Optional | YAML mapping for `gtag` configuration. Invalid YAML is skipped with a warning. |

---

#### Addendum: Google Analytics

Analytics is **disabled by default**. It is only emitted when `JEKYLL_ENV=production` and a valid `GA_MEASUREMENT_ID` is provided via secrets.

##### `GA_OPTIONS` Format
To ensure correct nesting, the value of the `GA_OPTIONS` secret must be stored as a **YAML mapping with a 2-space indentation** on every line. The rake task validates this format before writing `_config.secrets.yml`.

**Correct Secret Value:**
```text
  anonymize_ip: true
  cookie_expires: 0
```

#### Adding New Secrets

The build is prepared for new configs. To add one (for example, `MY_API_KEY`):

1. Create the secret in GitHub.
2. Add it to `.github/workflows/pages.yml` as an environment variable for the `config:secrets` step.
  ```yaml
  env:
    MY_API_KEY: ${{ secrets.MY_API_KEY }}
  ```
3. Extend `Rakefile` so `bundle exec rake config:secrets` validates and writes the new value.
4. Access it in templates via `{{ site.my_new_key }}`.


#### Local Testing

For local testing, you can run the same tasks used by GitHub Pages:

```bash
bundle exec rake assets:hashes
bundle exec rake config:secrets
bundle exec jekyll build --config _config.yml,_config.assets.yml,_config.secrets.yml
```

If you want to start a local server, use the Rake helper so the generated Sass partial exists before Jekyll compiles:

```bash
bundle exec rake serve
```

If you skip the asset generation step, `bundle exec jekyll serve` will fail with a Sass import error because `source/_sass/end2end/_fonts.sass` is generated, not committed.

If you want to test analytics locally, set environment variables before running `bundle exec rake config:secrets`:

```bash
GA_MEASUREMENT_ID=G-TEST \
GA_OPTIONS=$'anonymize_ip: true\ncookie_expires: 0' \
bundle exec rake config:secrets
```

You can still edit `_config.dev.yml` for regular local settings and use [Local Development](#local-development) instructions as usual.

If you need to inspect the generated secrets file manually, append `_config.secrets.yml` to the `--config` flag:

```bash
JEKYLL_ENV=production bundle exec jekyll serve --config _config.yml,_config.dev.yml,_config.secrets.yml
```

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

Install dependencies, generate the asset hashes, and start the local server with one command:

```bash
bundle exec rake serve
```

If you prefer to run Jekyll directly, generate the assets first so the Sass partial exists:

```bash
bundle exec rake assets:hashes
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

If you only need to install dependencies, run:

```bash
bundle install
```

Validate generated HTML and internal links:

```bash
rake check_links
```

### Cleaning build artifacts

Jekyll generates several cache and output directories during local development.

Over time these can cause stale assets, outdated caches, or inconsistent build results — especially after switching branches or pulling changes that affect CSS, fonts, or layouts.

If you notice unexpected output or slower builds, remove the generated artifacts before starting the server:

```bash
rm -rf _site .jekyll-cache .sass-cache .jekyll-metadata
```

- `_site` — the full generated site output; always rebuilt from source
- `.jekyll-cache` — internal Jekyll cache for incremental builds; can become stale after structural changes
- `.sass-cache` — Sass compilation cache; can cause outdated styles if asset files were renamed or moved
- `.jekyll-metadata` — tracks file modification times for incremental builds; removing it forces a clean full rebuild

These are all generated files. Removing them is safe and does not affect your source files.

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
  Please remove meta tags `<meta name="robots" content="noindex">` and `<meta name="googlebot" content="noindex">` in `source/_layouts/default.html`

Enjoy :yum:

#### Asset cache busting

We use content-based hashes (SHA-256) for cache busting, ensuring that browsers only download new assets when they actually change.

The `bundle exec rake assets:hashes` task generates both `_config.assets.yml` and the Sass partial used for the bundled icon font. The generated Sass lives at `source/_sass/end2end/_fonts.sass`, so Jekyll compiles it without Liquid inside the stylesheet.

To test this locally and generate the asset hash configuration, run:

> `bundle exec rake assets:hashes` regenerates `_config.assets.yml` and the generated font partial.
> The task validates that every source file exists before hashing it.
> The `main.css` hash also depends on the generated Sass partial, so the cache bust changes whenever the font definition changes.

```bash
bundle exec rake assets:hashes
```
