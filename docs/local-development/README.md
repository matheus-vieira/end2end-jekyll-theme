# Local development

This page documents the recommended local development workflow for this
theme. Keep it short — the `rake` helper takes care of dependency install and
asset generation so you can run the server reliably.

Recommended (single-command)
----------------------------

Start a local server and generate required assets in one step:

```bash
bundle exec rake serve
```

Notes:
- `rake serve` runs `bundle install`, generates the WOFF2/font partial and
  `_config.assets.yml`, then starts `jekyll serve` with the dev config.
- This is the preferred workflow for contributors and maintainers.

Direct Jekyll (when needed)
----------------------------

If you must run Jekyll directly, generate the assets first:

```bash
bundle exec rake assets:hashes
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

Testing analytics or secrets locally
----------------------------------

Generate `_config.secrets.yml` from environment variables when you need
production-like settings locally:

```bash
GA_MEASUREMENT_ID=G-TEST \
GA_OPTIONS=$'anonymize_ip: true\ncookie_expires: 0' \
bundle exec rake config:secrets
```

Cleaning build artifacts
------------------------

If you need a clean build, remove generated files before starting:

```bash
rm -rf _site .jekyll-cache .sass-cache .jekyll-metadata
```

Troubleshooting
---------------
- If Jekyll fails due to missing Sass partials, run `bundle exec rake assets:hashes`.
- If comments or other third-party widgets don't load, verify `site.comments`
  config in `_config.yml` or `_config.secrets.yml` when using secrets.
