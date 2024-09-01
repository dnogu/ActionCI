# Docker Publish Workflow - `docker-publish.yaml`

This workflow automates the process of building, tagging, and publishing Docker images to a registry. It includes steps for setting up Docker, building images with metadata, and pushing them to the specified container registry, ensuring a streamlined and consistent deployment process.

## Workflow Overview

```yaml
name: Docker Publish Module

on:
  push:
    tags: [ 'v*.*.*' ]
  pull_request:
    branches: [ "main" ]

jobs:
  release:
    uses: dnogu/ActionCI/.github/workflows/docker-publish.yaml@main
    secrets: inherit

```
### Key Features

- **Automated Docker Builds**: Automatically builds and tags Docker images with metadata extracted from the repository.
- **Registry Integration**: Supports pushing to Docker Hub or GitHub Container Registry.
- **Secure Signing**: Integrates with Sigstore Cosign for secure signing of Docker images.

## How to Use

1. **Add this workflow** to your repository under `.github/workflows/`.
2. **Customize** the environment variables and steps as needed to match your registry and project requirements.

## Credits

Special thanks to the following contributors who developed the components used in the `docker-publish.yaml` workflow:

- **GitHub Actions Team**: For the official `Checkout` action, which facilitates repository access.
  - Action used:
    - [`actions/checkout@v3`](https://github.com/actions/checkout)
- **Docker Team**: For the Docker-specific actions that enable advanced Docker operations, including building, logging in, and metadata extraction.
  - Actions used:
    - [`docker/setup-buildx-action@v3.0.0`](https://github.com/docker/setup-buildx-action)
    - [`docker/login-action@v3.0.0`](https://github.com/docker/login-action)
    - [`docker/metadata-action@v5.0.0`](https://github.com/docker/metadata-action)
    - [`docker/build-push-action@v5.0.0`](https://github.com/docker/build-push-action)
- **Sigstore Team**: For providing the Cosign installer, enabling secure signing of Docker images.
  - Action used:
    - [`sigstore/cosign-installer@v3.3.0`](https://github.com/sigstore/cosign-installer)

Your contributions have been invaluable to the success of this project.

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
