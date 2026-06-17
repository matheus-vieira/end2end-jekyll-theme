# Comments subsystem

This section documents the comments implementation used by the theme. The
theme supports multiple providers via the `comments.provider` configuration in
`_config.yml`. Available options:

- `giscus` (default) — GitHub Discussions-based comments via the giscus widget.
- `disqus` — legacy Disqus integration (kept as optional fallback).
- `false` — disable comments rendering.

Per-page enablement: set `comments: true` in the post front matter to render
the comments block for a specific page.

Configuration reference and provider-specific setup:
- `giscus/` — setup and example config
- `disqus/` — legacy usage and migration guidance
