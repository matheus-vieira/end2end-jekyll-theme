# Contributing

Thanks for contributing to this repository.

## Branch strategy

- `main` is protected and only accepts changes through pull requests.
- `epic/*` branches are protected and only accept changes through pull requests.
- Releases should be created from Git tags.

## Contribution workflow

1. Create a branch from `main`.
2. Make your changes.
3. Run local validation.
4. Open a pull request to the correct base branch.
5. Use a Conventional Commits title for the pull request.
6. Merge using squash merge.

## Commit and pull request title format

Use Conventional Commits for branch commits and pull request titles.

Examples:

- `feat: add related posts section`
- `fix: correct mobile menu spacing`
- `docs: update local development guide`
- `chore: rename workflow files`

## Local validation

```bash
bundle install
bundle exec rake check_links
bundle exec jekyll build --config _config.yml
```

## Pull request checklist

- The pull request title follows Conventional Commits.
- The change targets the correct base branch.
- Local validation was executed.
- Documentation was updated when needed