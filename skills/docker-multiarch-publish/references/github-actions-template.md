# GitHub Actions Template: Multi-Arch Docker Publish (OCI Registry)

```yaml
env:
  REGISTRY: ghcr.io # or docker.io
  IMAGE_REPO: ${{ github.repository_owner }}/vmemo # for docker.io use "<namespace>/<image>"
  IMAGE_NAME: ${{ env.REGISTRY }}/${{ env.IMAGE_REPO }}

jobs:
  build_by_arch:
    strategy:
      fail-fast: false
      matrix:
        include:
          - arch: amd64
            platform: linux/amd64
            runner: ubuntu-24.04
          - arch: arm64
            platform: linux/arm64
            runner: ubuntu-24.04-arm
    runs-on: ${{ matrix.runner }}
    steps:
      - uses: actions/checkout@v6

      - uses: docker/setup-buildx-action@v4

      # Docker Hub example:
      # - uses: docker/login-action@v4
      #   with:
      #     username: ${{ vars.DOCKERHUB_USERNAME }}
      #     password: ${{ secrets.DOCKERHUB_TOKEN }}
      #
      # GHCR example:
      - uses: docker/login-action@v4
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - uses: docker/build-push-action@v7
        with:
          context: .
          file: ./Dockerfile
          push: true
          platforms: ${{ matrix.platform }}
          tags: ${{ env.IMAGE_NAME }}:${{ env.TARGET_TAG }}-${{ matrix.arch }}

  push_image:
    runs-on: ubuntu-latest
    needs: [build_by_arch]
    steps:
      - uses: docker/setup-buildx-action@v4

      # Keep login config aligned with selected registry.
      - uses: docker/login-action@v4
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Merge arch tags into multi-platform manifest
        run: |
          set -euo pipefail
          docker buildx imagetools create \
            -t "${IMAGE_NAME}:${TARGET_TAG}" \
            "${IMAGE_NAME}:${TARGET_TAG}-amd64" \
            "${IMAGE_NAME}:${TARGET_TAG}-arm64"

      - name: Verify merged manifest
        run: |
          set -euo pipefail
          docker buildx imagetools inspect "${IMAGE_NAME}:${TARGET_TAG}"

      - name: Cleanup temporary arch tags
        run: |
          # Implement registry-specific cleanup logic for:
          # ${IMAGE_NAME}:${TARGET_TAG}-amd64 and ${IMAGE_NAME}:${TARGET_TAG}-arm64
          # Docker Hub often uses hub API/delete scripts.
          # GHCR can use gh api or oras/crane workflows.
          echo "cleanup temp tags"
```

## Optional Alias Policy Example

- Prerelease: publish `stag`
- Stable release and confirmed latest release: publish `latest`
- Otherwise: skip alias updates

## Notes

- For GHCR, ensure workflow permissions include `packages: write`.
- For Docker Hub, use a Docker Hub access token in secrets.
- Keep `IMAGE_NAME` registry-prefixed to avoid accidental pushes to wrong registry.
