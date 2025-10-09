# GitHub Actions and Workflows

A collection of GitHub composite actions and reusable workflows.

## Features

- [Reusable Megalinter Workflow](#reusable-megalinter-workflow)
- [Reusable Semantic Release Workflow](#reusable-semantic-release-workflow)
- [Sync Files Workflow](#sync-files-workflow)

### Reusable Megalinter Workflow

Reusable [Megalinter workflow](.github/workflows/megalinter.yaml) using Megalinter flavor Documentation to lint, format, report, and apply fixes. On push to non-main branch generates Megalinter report, on a PR addiotionaly creates a commit with fixes, on push to main creates a PR with fixes.
Usage, for example, `.github/workflow/megalinter.yaml`:

```yaml
---
name: Megalinter

on:
  push:
  pull_request:
    branches:
      - main

jobs:
  megalinter:
    name: Megalinter
    uses: xebis/github-actions-and-workflows/.github/workflows/megalinter.yaml@v0
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> [!note]
> To use GitHub App instead of GitHub token pass`inputs.GH_APP_ID` and `secrets.GH_APP_PEM_FILE` instead of `secrets.GITHUB_TOKEN`.

Configure in a repository, for example, [`.mega-linter.yml`](.mega-linter.yml), [`.markdownlint.yaml`](.markdownlint.yaml), and [`.markdownlintignore`](.markdownlintignore).

Also set up GitHub workflow permissions to allow commits and pull requests from workflows:

- GitHub
  - _Organization_ -> Settings -> Actions -> General
    - Workflow permissions: Read and write permissions
      - Allow GitHub Actions to create and approve pull requests: On
  - _Repository_ -> Settings -> Actions -> General
    - Workflow permissions: Read and write permissions
      - Allow GitHub Actions to create and approve pull requests: On

Local Mgalinter run: `sudo docker run --rm -u $(id -u):$(id -g) -v /var/run/docker.sock:/var/run/docker.sock:rw -v $(pwd):/tmp/lint:rw oxsecurity/megalinter-documentation:v8` and fix root created files: `sudo chown -R $(id -u):$(id -g) .`

### Reusable Semantic Release Workflow

Reusable [Semantic Release workflow](.github/workflows/semantic-release.yaml) using the Conventional Commits preset to automate versioning, generates [GitHub releases](https://github.com/xebis/github-actions-and-workflows/releases), updates the [CHANGELOG](CHANGELOG.md), and creates or updates major tag.  
Usage, for example, `.github/workflows/semantic-release.yaml`:

```yaml
---
name: Semantic Release

on:
  push:
    branches:
      - main

jobs:
  release:
    name: Release
    uses: xebis/github-actions-and-workflows/.github/workflows/semantic-release.yaml@v0
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> [!note]
> To use GitHub App instead of GitHub token pass`inputs.GH_APP_ID` and `secrets.GH_APP_PEM_FILE` instead of `secrets.GITHUB_TOKEN`. See [`examples/workflows/semantic-release.yaml`](examples/workflows/semantic-release.yaml).

Configure in a repository, for example, `.releaserc.yaml`:

```yaml
---
branches:
  - main
plugins:
  - - "@semantic-release/commit-analyzer"
    - preset: conventionalcommits
  - - "@semantic-release/release-notes-generator"
    - preset: conventionalcommits
  - "@semantic-release/github"
  - - "@semantic-release/changelog"
    - changelogTitle: '# Changelog'
  - - "@semantic-release/git"
    - assets:
      - CHANGELOG.md
  - semantic-release-major-tag
```

### Sync Files Workflow

[Sync Files Workflow](.github/workflows/sync-files.yaml) synchronizes the repository contents by creating a pull request to the target repositories.

Configure target repositories:

- Target repositories are specified at the source repository [Sync Files Workflow](.github/workflows/sync-files.yaml) in the matrix.
- Files to sync are specified in the target repository file `.github/sync.yaml`:

  ```yaml
  ---
  copy:
  - .github/workflows/semantic-release.yaml
  - .releaserc.yaml
  ```

## Credits and Acknowledgments

- Martin Bružina - Author

## Copyright and Licensing

- MIT License  
  Copyright © 2025 Martin Bružina
