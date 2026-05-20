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

### Start in 4 steps

> Ruby >= 3.2 required.
>\
> Bundler 4.0.11 recommended

1. Download or clone repo `git clone git@github.com:nandomoreirame/end2end.git`
2. Enter the folder: `cd end2end/`
3. Install Ruby gems: `bundle install`
4. Start Jekyll server: `bundle exec jekyll serve --config _config.yml,_config.dev.yml`

Access, [localhost:4000/end2end](http://localhost:4000/end2end)

### Deploy on GitHub Pages

Deployment is handled by GitHub Actions, so no manual publish step is required.

1. In **Settings → Pages**, set **Source** to **GitHub Actions**.
2. Push your changes to `master` (or the repository default branch).
3. Wait for the **Deploy GitHub Pages** workflow to finish publishing the site.

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
