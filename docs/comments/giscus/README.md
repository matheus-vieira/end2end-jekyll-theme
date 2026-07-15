# giscus (GitHub Discussions) setup

Required site config (add to `_config.yml` under `comments.giscus`):

```yaml
comments:
  provider: giscus
  giscus:
    repo: owner/repo            # e.g. matheus-vieira/end2end-jekyll-theme
    repo_id: <REPO_ID>
    category: Discussions       # discussion category name
    category_id: <CATEGORY_ID>
    mapping: pathname           # options: pathname, url, title, og:title
    reactions: "1"
    emit_metadata: "0"
    input_position: bottom
    theme: light
```

How to obtain IDs:
- `repo_id` and `category_id` can be obtained via the GitHub API or from
  the giscus configuration UI when setting up the widget.

Notes:
- The theme lazy-loads the giscus script when the comments block enters the
  viewport to avoid loading third-party scripts on every page load.
- For local testing, set `comments.provider: giscus` and provide the required
  IDs. If you don't set them, the component will render but giscus may fail
  to initialize.
