# GitHub Actions and Workflows

A collection of GitHub composite actions and reusable workflows.

## Features

- Reusable [Semantic Release workflow](.github/workflows/semantic-release.yaml) using the Conventional Commits preset to automate versioning, generates [GitHub releases](https://github.com/xebis/github-actions-and-workflows/releases), updates the [CHANGELOG](CHANGELOG.md), and creates or updates major tag.  
  Use in a repository from a workflow, for example, `.github/workflows/semantic-release.yaml`:

  ```yaml
  name: Semantic Release

  on:
    push:
      branches:
        - main

  jobs:
    call-reusable-release:
      uses: xebis/github-actions-and-workflows/.github/workflows/semantic-release.yml@main
      secrets:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ```

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

## Credits and Acknowledgments

- Martin Bružina - Author

## Copyright and Licensing

- MIT License  
  Copyright © 2025 Martin Bružina
