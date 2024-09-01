# Lint Workflow - `tflint.yaml`

This workflow automates the process of linting Terraform configurations across different operating systems (Ubuntu, macOS, and Windows) using TFLint. It ensures consistent code quality and adherence to best practices by running TFLint checks on your Terraform codebase.

## Workflow Overview

```yaml
name: TFLINT Module

on:
  pull_request:
    branches:
      - main

jobs:
  release:
    uses: dnogu/ActionCI/.github/workflows/tflint.yaml@main
    secrets: inherit

```
### Key Features

- **Cross-Platform Support**: Runs on Ubuntu, macOS, and Windows to ensure consistent linting across environments.
- **TFLint Setup**: Automatically installs and configures TFLint, ensuring the correct version and plugins are used.
- **Caching**: Caches the TFLint plugin directory to speed up subsequent runs.
- **Version Control**: Displays the TFLint version used in the workflow for transparency and troubleshooting.

## How to Use

1. **Add this workflow** to your repository under `.github/workflows/`.
2. **Customize** the `.tflint.hcl` configuration file in `.github/config/` as needed for your project.
3. **Invoke the workflow** as part of your CI/CD process to maintain Terraform code quality.

## Credits

Special thanks to the following contributors who developed the components used in the `tflint.yaml` workflow:

- **GitHub Actions Team**: For the official `Checkout` and `Cache` actions, which facilitate repository access and caching.
  - Actions used:
    - [`actions/checkout@v4`](https://github.com/actions/checkout)
    - [`actions/cache@v4`](https://github.com/actions/cache)
- **Terraform Linters Team**: For the `setup-tflint` action, which simplifies the installation and setup of TFLint.
  - Action used:
    - [`terraform-linters/setup-tflint@v4`](https://github.com/terraform-linters/setup-tflint)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
