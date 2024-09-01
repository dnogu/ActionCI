# Docker Publish Workflow - `docker-publish.yaml`

This workflow automates the process of building, tagging, and publishing Docker images to a registry. It includes steps for setting up Docker, building images with metadata, and pushing them to the specified container registry, ensuring a streamlined and consistent deployment process.

## Workflow Overview

```yaml
name: Docker

on:
  workflow_call:

env:
  # Use docker.io for Docker Hub if empty
  REGISTRY: ghcr.io
  # github.repository as <account>/<repo>
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      id-token: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Install cosign (conditional on event)
        if: github.event_name != 'pull_request'
        uses: sigstore/cosign-installer@6e04d228eb30da1757ee4e1dd75a0ec73a653e06 # Specific commit hash for v3.1.1
        with:
          cosign-release: 'v2.1.1'

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@f95db51fddba0c2d1ec667646a06c2ce06100226 # v3.0.0

      - name: Log into registry ${{ env.REGISTRY }}
        if: github.event_name != 'pull_request'
        uses: docker/login-action@343f7c4344506bcbf9b4de18042ae17996df046d # v3.0.0
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract Docker metadata
        id: meta
        uses: docker/metadata-action@96383f45573cb7f253c731d3b3ab81c87ef81934 # v5.0.0
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=semver,pattern={{version}}
            type=ref,event=tag,value=latest

      - name: Build and push Docker image
        id: build-and-push
        uses: docker/build-push-action@0565240e2d4ab88bba5387d719585280857ece09 # v5.0.0
        with:
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          platforms: linux/amd64,linux/arm64
          cache-from: type=gha
          cache-to: type=gha,mode=max

      # New steps for installing and checking Cosign installation
      - name: Install Cosign
        if: github.event_name != 'pull_request'
        uses: sigstore/cosign-installer@v3.3.0

      - name: Check Cosign installation
        if: github.event_name != 'pull_request'
        run: cosign version
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
