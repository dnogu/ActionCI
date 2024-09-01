# `release.yaml` - Release Module Workflow

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

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
