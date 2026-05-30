# Rake tasks

This file documents the common `Rakefile` tasks used during development and
deployment.


Common tasks (short reference):

- `rake serve` — installs deps, generates assets, and starts a local server.
- `rake assets:hashes` — generate asset hashes and font partial.
- `rake config:secrets` — build `_config.secrets.yml` from env vars.
- `rake post` — create a new post with front-matter.
- `rake check_links` — validate generated HTML and internal links.

See the short task pages and examples under:

- [docs/rake/tasks/serve/](docs/rake/tasks/serve/)
- [docs/rake/tasks/assets-hashes/](docs/rake/tasks/assets-hashes/)
- [docs/rake/tasks/config-secrets/](docs/rake/tasks/config-secrets/)
- [docs/rake/tasks/post/](docs/rake/tasks/post/)
- [docs/rake/tasks/check-links/](docs/rake/tasks/check-links/)

For detailed usage examples, inspect the `Rakefile` and the tasks' help
messages (`rake -T`).
