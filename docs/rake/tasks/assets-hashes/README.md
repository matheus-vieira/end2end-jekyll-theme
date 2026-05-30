# `rake assets:hashes`

Generates the `_config.assets.yml` asset hash file and the generated Sass
partial used for self-hosted fonts. Run this before production builds so the
cache-busted asset names are available.

Usage:

```bash
bundle exec rake assets:hashes
```
