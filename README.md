# end2end Clean Jekyll theme [![Deploy Pages](https://github.com/nandomoreirame/end2end/actions/workflows/pages.yml/badge.svg?branch=master)](https://github.com/nandomoreirame/end2end/actions/workflows/pages.yml)



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

## Current stack

- Ruby 3.2+
- Jekyll 4.3
- GitHub Pages via GitHub Actions
- Sass modules using `@use`

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

## Deploy

Deploy is fully handled by GitHub Actions.

Requirements:

- Repository Settings
- Pages
- Source = GitHub Actions

Pushes to `master` or `main` trigger deployment automatically.

## Migration notes

This project no longer uses:

- Travis CI
- `gh-pages` branch deployment
- Bourbon Sass framework

The site is now built and deployed using GitHub Actions artifacts.

---

### Using Rake tasks

* Create a new page: `rake page name="contact.md"`
* Create a new post: `rake post title="TITLE OF THE POST"`

---

### Demo and Download

[Demo](https://nandomoreirame.github.io/end2end/)
[Download](https://github.com/nandomoreirame/end2end/archive/master.zip)

![end2end - free Jekyll theme](/screenshot.png)

---

### Copyright and license

It is under [the MIT license](/LICENSE).

> :warning:
  Please remove metas `<meta name="robots" content="noindex">` and `<meta name="googlebot" content="noindex">` in `source/_layouts/default.html`

Enjoy :yum:
