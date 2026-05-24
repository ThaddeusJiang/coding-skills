# GitHub Actions Template: Multi-Arch Docker Publish

```yaml
name: Docker Publish

on:
  workflow_call:
    inputs:
      version:
        description: "Semver version to tag (e.g. 1.2.3)"
        type: string
        required: true

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Docker metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/owner/image
          tags: |
            type=raw,value=latest
            type=raw,value=${{ inputs.version }},enable=${{ inputs.version != '' }}

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          platforms: linux/amd64,linux/arm64
          provenance: false
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

## Notes

- This template follows the same shape as `typelens` and is intentionally single-job.
- Replace `ghcr.io/owner/image` with the real target image name.
- Keep login configuration aligned with the target registry.
