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
    uses: dnogu/ActionCI/.github/workflows/opentofu-apply.yaml@latest
    secrets: inherit
    with:
      TF_ACTIONS_WORKING_DIR: ${{ vars.TF_ACTIONS_WORKING_DIR }}
      OPEN_TOFU_VER: ${{ vars.OPEN_TOFU_VER }}
      OIDC_AWS: True
      AWS_AUTH_REGION: 'us-east-1'
      AWS_ROLE_TO_ASSUME: ${{ secrets.AWS_ROLE_TO_ASSUME }}
      AWS_ROLE_SESSION_NAME: ${{ secrets.AWS_ROLE_SESSION_NAME }}
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
      OIDC_AWS:
        required: true
        type: boolean
        default: false
      AWS_AUTH_REGION:
        required: false
        type: string
        default: "us-east-1"
      AWS_ROLE_TO_ASSUME:
        required: false
        type: string
        default: "placeholder_role"
      AWS_ROLE_SESSION_NAME:
        required: false
        type: string
        default: "MySessionName"

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

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        if: ${{ inputs.OIDC_AWS == true }}
        with:
          aws-region: ${{ inputs.AWS_AUTH_REGION }}
          role-to-assume: ${{ inputs.AWS_ROLE_TO_ASSUME }}
          role-session-name: ${{ inputs.AWS_ROLE_SESSION_NAME }}

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

      - run: mkdir -p uploads

      - name: OpenTofu Plan
        id: plan
        run: tofu plan -no-color -out=uploads/tfplan.json
        continue-on-error: true

      - name: Upload TF Plan Artifact
        uses: actions/upload-artifact@v4
        with:
          name: tfplan
          path: terraform/uploads/tfplan.json

      - run: mkdir -p downloads

      - name: Download TF Plan Artifact
        uses: actions/download-artifact@v4
        with:
          name: tfplan
          path: terraform

      - name: Display structure of downloaded files
        run: ls -R

      - name: OpenTofu Apply
        # if: github.event_name == 'workflow_dispatch'
        run: tofu apply -no-color tfplan.json

```

## Credits

Special thanks to the following contributors who developed the components used in the `opentofu-plan.yaml` workflow:

- **GitHub Actions Team**: For the official `Checkout` action, facilitating repository access.
  - Action used:
    - [`actions/checkout@v3`](https://github.com/actions/checkout)
- **AWS for GitHub Actions**: For providing the setup action that authenticates GitHub Actions to AWS via OIDC.
  - Action used:
    - [`aws-actions/configure-aws-credentials@v4`](https://github.com/marketplace/actions/configure-aws-credentials-action-for-github-actions)
- **OpenTofu Team**: For providing the setup action that integrates OpenTofu with GitHub Actions.
  - Action used:
    - [`opentofu/setup-opentofu@v1`](https://github.com/opentofu/setup-opentofu)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
