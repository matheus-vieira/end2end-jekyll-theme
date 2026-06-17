# `rake config:secrets`

Builds `_config.secrets.yml` from environment variables with validation. Use
this when running production builds that require secrets (analytics IDs,
etc.). The generated file is not committed to the repository.

Usage example:

```bash
GA_MEASUREMENT_ID=G-XXXX bundle exec rake config:secrets
```
