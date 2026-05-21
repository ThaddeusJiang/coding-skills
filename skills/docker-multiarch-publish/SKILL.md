---
name: "docker-multiarch-publish"
description: "Build and publish Docker images for both amd64 and arm64 to OCI registries (Docker Hub, GHCR, and compatible registries) by splitting per-arch builds and merging with a multi-platform manifest."
---

# Docker Multi-Arch Publish Skill

Use this skill when a repository needs Docker images published for both `amd64` and `arm64`.

## Goal

Provide a repeatable CI workflow that:

- Builds and pushes `amd64` and `arm64` images independently.
- Creates a single multi-platform manifest tag from temporary arch tags.
- Optionally updates `latest`/`stag`-like aliases under controlled conditions.
- Verifies pushed manifests before cleanup.

Supported registries:

- Docker Hub
- GitHub Container Registry (`ghcr.io`)
- Other OCI-compatible registries

## When To Use

- Existing workflow only publishes single architecture.
- Project needs to run on both x86_64 cloud and ARM devices/instances.
- You want deterministic publish behavior without business-layer complexity.
- Project publishes to Docker Hub, GHCR, or another OCI registry.

## Required Workflow Pattern

1. Build per architecture in matrix jobs.
2. Push temporary arch tags:
   - `<image>:<tag>-amd64`
   - `<image>:<tag>-arm64`
3. Merge tags with `docker buildx imagetools create` into:
   - `<image>:<tag>`
4. Conditionally create alias tags (example: `latest`, `stag`) only when rules pass.
5. Verify each created manifest using `docker buildx imagetools inspect`.
6. Remove temporary arch tags from registry.

## Key Design Choices

- Prefer **native runners per architecture** (example: `ubuntu-24.04` + `ubuntu-24.04-arm`) to avoid heavy QEMU emulation overhead.
- Keep release/tag decision logic in CI script; keep application code unaware of image publish policy.
- Use temporary arch tags as an explicit merge boundary to simplify retries and troubleshooting.
- Keep registry concerns isolated in CI variables (`REGISTRY`, `IMAGE_REPO`, auth secrets).

## Minimal Snippet

Reference file:

- `references/github-actions-template.md`

## Acceptance Checklist

- [ ] Workflow publishes both `<tag>-amd64` and `<tag>-arm64`.
- [ ] Workflow publishes merged `<tag>` manifest containing both architectures.
- [ ] Alias tags are only created when policy allows.
- [ ] Manifest verification steps pass for all created tags.
- [ ] Temporary arch tags are cleaned up.

## Guardrails

- Do not introduce custom retry loops or sleep/backoff in business code for image publishing.
- Do not skip manifest inspection after create.
- Do not overwrite `latest` unless current release is confirmed as repository latest stable release.
- Do not hardcode Docker Hub-only login/cleanup logic when target registry is GHCR or other OCI registry.
