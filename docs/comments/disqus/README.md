# Disqus (legacy) notes and migration

To enable Disqus as the provider, set in `_config.yml`:

```yaml
comments:
  provider: disqus
  disqus:
    shortname: your-disqus-shortname
```

Migration note:
- Existing Disqus comments are NOT migrated automatically by the theme.
- Recommended path: export comments from Disqus (Data Export) and import
  them into GitHub Discussions using the available import tools or scripts.

Privacy:
- Disqus injects third-party scripts and may load trackers or ads depending
  on the account. That's why `giscus` is the preferred default for this
  project.
