# GitHub Actions and Workflows

A collection of GitHub composite actions and reusable workflows.

## Features

- Reusable [Semantic Release workflow](.github/workflows/semantic-release.yaml) using the Conventional Commits preset to automate versioning, generates [GitHub releases](https://github.com/xebis/github-actions-and-workflows/releases), and update the [CHANGELOG](CHANGELOG.md).  
  Usage:  

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

## Credits and Acknowledgments

- Martin Bružina - Author

## Copyright and Licensing

- MIT License  
  Copyright © 2025 Martin Bružina
