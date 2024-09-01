# Release Module Workflow - `release.yaml` 

This workflow automates the release process for the ActionCI module. It is triggered when a pull request to the `main` branch is closed, ensuring that releases are handled efficiently and consistently.

## Workflow Overview

```yaml
name: Release Module

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  release:
    uses: dnogu/ActionCI/.github/workflows/release.yaml@main
    secrets: inherit
```
### Key Features

- **Trigger**: Automatically runs on closed pull requests to the `main` branch.
- **Modular Design**: Leverages the `release.yaml` reusable workflow.
- **Secure**: Inherits secrets to manage sensitive information securely.

## How to Use

1. **Add this workflow** to your repository under `.github/workflows/`.
2. **Customize** the `release.yaml` file if needed to fit your specific release process.

## Credits

Special thanks to the following contributors who developed the components used in the `release.yaml` workflow:

- **WyriHaximus**: For the `Get Previous Tag` and `Get Next Version` GitHub Actions, which handle versioning in the workflow.
  - Actions used:
    - [`WyriHaximus/github-action-get-previous-tag@v1`](https://github.com/WyriHaximus/github-action-get-previous-tag)
    - [`WyriHaximus/github-action-next-semvers@v1`](https://github.com/WyriHaximus/github-action-next-semvers)
- **GitHub Actions Team**: For the official `Checkout` and `Create Release` actions, which facilitate repository access and release creation.
  - Actions used:
    - [`actions/checkout@v4`](https://github.com/actions/checkout)
    - [`actions/create-release@v1`](https://github.com/actions/create-release)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
