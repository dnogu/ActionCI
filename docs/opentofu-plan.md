# OpenTofu Plan Workflow - `opentofu-plan.yaml`

This workflow automates the setup and execution of OpenTofu for Terraform-based projects. It handles formatting, initialization, validation, and planning steps to ensure that your infrastructure code is correct and ready for deployment.

## How to Use

1. **Add this workflow** to your repository under `.github/workflows/`.
2. **Customize** the `opentofu-plan.yaml` file if needed to fit your specific Terraform setup.

### Example Usage

```yaml
on:
  pull_request:
    branches: '*'

jobs:
  opentofu:
    uses: dnogu/ActionCI/.github/workflows/opentofu-plan.yaml@774a837107a86a489e92798a1fb571544c9e00ff
    secrets: inherit
    with:
      TF_ACTIONS_WORKING_DIR: ${{ vars.TF_ACTIONS_WORKING_DIR }}
      OPEN_TOFU_VER: ${{ vars.OPEN_TOFU_VER }}
```
### Key Features

- **OpenTofu Setup**: Automatically installs and configures OpenTofu for your Terraform environment.
- **Cross-Platform Terraform Execution**: Performs formatting checks, initialization, validation, and planning.
- **Customizable**: Supports configurable working directories and OpenTofu versions via workflow inputs.

## Workflow Overview

```yaml
name: OpenTofu

on:
  workflow_call:
    inputs:
      TF_ACTIONS_WORKING_DIR:
        required: false
        type: string
        default: "./terraform"
      OPEN_TOFU_VER:
        required: false
        type: string
        default: "latest"

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      id-token: write
    defaults:
      run:
        working-directory: ${{ inputs.TF_ACTIONS_WORKING_DIR }}

    steps:
    - uses: actions/checkout@v3
    - uses: opentofu/setup-opentofu@v1
      with:
        tofu_version: ${{ inputs.OPEN_TOFU_VER }}

    - name: OpenTofu fmt
      id: fmt
      run: tofu fmt -check
      continue-on-error: true

    - name: OpenTofu Init
      id: init
      run: tofu init

    - name: OpenTofu Validate
      id: validate
      run: tofu validate -no-color

    - name: OpenTofu Plan
      id: plan
      run: tofu plan -no-color
      continue-on-error: true
```

## Credits

Special thanks to the following contributors who developed the components used in the `opentofu-plan.yaml` workflow:

- **GitHub Actions Team**: For the official `Checkout` action, facilitating repository access.
  - Action used:
    - [`actions/checkout@v3`](https://github.com/actions/checkout)
- **OpenTofu Team**: For providing the setup action that integrates OpenTofu with GitHub Actions.
  - Action used:
    - [`opentofu/setup-opentofu@v1`](https://github.com/opentofu/setup-opentofu)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
