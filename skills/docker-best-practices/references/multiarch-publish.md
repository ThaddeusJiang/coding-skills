# Multi-Arch Publish

## Goal

Publish Docker images for both `amd64` and `arm64` from a single Buildx workflow run.

## Required Pattern

1. Use a single job with `docker/setup-qemu-action` and `docker/setup-buildx-action`.
2. Log in to the target registry explicitly.
3. Generate tags and labels with `docker/metadata-action`.
4. Build and push `linux/amd64,linux/arm64` together with `docker/build-push-action`.
5. Let Buildx publish the multi-platform manifest directly under the final tags.

## Runner and Registry Notes

- `ubuntu-latest` is sufficient for this pattern when QEMU plus Buildx is acceptable.
- Keep registry configuration explicit in workflow inputs or action configuration.
- For GHCR, ensure workflow permissions include `packages: write`.
- For Docker Hub, use a scoped access token.

## Tagging Pattern

- Use `docker/metadata-action` to generate canonical tags.
- Keep alias tags such as `latest` explicit in metadata rules.
- Add version tags from workflow inputs or release metadata instead of hand-building strings in shell steps.

## Guardrails

- Do not add custom retry or backoff logic into business code for image publishing.
- Do not split into per-arch temporary tags unless there is a concrete constraint requiring that extra complexity.
- Do not hardcode Docker Hub-only behavior when the target is GHCR or another OCI registry.
- Do not scatter tag construction across shell steps when `docker/metadata-action` can own that logic.
- Do not enable provenance flags blindly; follow the repository's existing publish policy.

## Acceptance Checklist

- Workflow publishes one multi-platform image containing both `linux/amd64` and `linux/arm64`.
- Alias tags are updated only when policy conditions pass.
- Tags and labels are produced from CI metadata configuration.
- Registry login and permissions are explicit in the workflow.

## Reference Workflow

This guidance follows the structure used by `typelens`:

- https://github.com/copperline-ai/typelens/blob/main/.github/workflows/docker-publish.yml
