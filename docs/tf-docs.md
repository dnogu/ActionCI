# Generate Terraform Docs Workflow - `tf-docs.yaml`

This workflow automates the process of generating and injecting Terraform documentation directly into the `README.md` file. It runs upon invocation and is designed to keep the documentation in sync with the Terraform configuration.

## Workflow Overview

```yaml
name: TF-Docs Module

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  release:
    uses: dnogu/ActionCI/.github/workflows/tf-docs.yaml@main
    secrets: inherit

```
### Key Features

- **Automated Documentation**: Automatically generates and injects Terraform documentation into the `README.md`.
- **Seamless Integration**: Updates are pushed directly to the PR branch, keeping documentation up-to-date with the latest changes.
- **Customizable**: Supports custom configurations through the `.github/config/terraform-docs.yaml` file.

## How to Use

1. **Add this workflow** to your repository under `.github/workflows/`.
2. **Ensure your Terraform configuration** is located in the appropriate directory and customize the `.github/config/terraform-docs.yaml` as needed.
3. **Invoke the workflow** as part of your CI/CD process.

## Credits

Special thanks to the following contributors who developed the components used in the `tf-docs.yaml` workflow:

- **GitHub Actions Team**: For the official `Checkout` action, which facilitates repository access.
  - Action used:
    - [`actions/checkout@v3`](https://github.com/actions/checkout)
- **Terraform Docs Team**: For the `terraform-docs` GitHub Action, which automates the generation and injection of Terraform documentation.
  - Action used:
    - [`terraform-docs/gh-actions@v1.2.0`](https://github.com/terraform-docs/gh-actions)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
