---
name: docker-best-practices
description: Docker and Docker Compose best practices for reliable local/dev deployments and multi-arch image publishing. Use when writing or reviewing Dockerfiles, Compose files, or CI publish workflows.
license: MIT
metadata:
  author: amami
  version: "1.2.0"
---

# Docker Best Practices

Practical conventions for Docker, Docker Compose, and multi-arch publish workflows. The skill keeps high-level rules in this file and pushes implementation detail into focused reference documents.

## When to Use

Use this skill when tasks include:

- Adding or modifying `docker-compose.yml`
- Adding container health checks
- Configuring `depends_on` startup order
- Reviewing Docker runtime reliability in local/dev environments
- Building and publishing images for both `amd64` and `arm64`
- Creating or reviewing GitHub Actions workflows for OCI registries

## Rule Categories

### 1. Compose Runtime Reliability

- `healthcheck-command-availability`: In Docker Compose `healthcheck.test`, do not use commands that are not present in the image.
- `healthcheck-over-fragility`: If no reliable built-in command exists, skip the healthcheck instead of adding tool-install complexity.
- `dependency-healthcheck-gating`: Use `depends_on: condition: service_healthy` only when the upstream image has a valid executable healthcheck.

### 2. Multi-Arch Publish Workflow

- `multiarch-buildx-direct-publish`: Prefer a single Buildx multi-platform push over split arch jobs unless there is a proven runner or registry constraint.
- `publish-policy-separation`: Keep release alias policies (`latest`, `stag`) in CI workflow logic, not in application code.
- `metadata-driven-tagging`: Use workflow metadata generation for image tags and labels instead of hand-built tag strings when possible.
- `registry-auth-explicit`: Registry login and package permissions must be explicit in the workflow.

### 3. Base Image Selection

- `anti-alpine-slim`: Unless there is a specific requirement, do not use alpine or slim base images. Prefer the default stable image tag.

## How to Apply

1. Verify available binaries in the target image before writing `healthcheck.test`.
2. If command availability is uncertain and adding tools would increase complexity, skip healthcheck instead of adding fragile checks.
3. Use `depends_on: condition: service_healthy` only when the dependency service has a valid and executable healthcheck.
4. For multi-arch publish, prefer one Buildx job that pushes `linux/amd64,linux/arm64` together.
5. Use `docker/metadata-action` or equivalent logic to generate tags and labels in CI.
6. Unless the task has an explicit footprint or compatibility requirement, use the default stable base image instead of `alpine` or `slim` variants.
7. Run `docker compose config` after Compose edits to validate syntax and merged configuration.

## Reference Files

- `references/compose-healthchecks.md`: healthcheck design and validation rules
- `references/compose-dependencies.md`: `depends_on` and readiness gating guidance
- `references/multiarch-publish.md`: publish flow, guardrails, and acceptance checks
- `references/github-actions-multiarch-template.md`: minimal GitHub Actions template

## Acceptance Checklist

- [ ] Compose changes pass `docker compose config`.
- [ ] No service depends on a broken or non-executable healthcheck.
- [ ] CI publishes one multi-platform image containing both `amd64` and `arm64`.
- [ ] Alias tags are updated only when policy conditions pass.
- [ ] Tag and label generation are derived from CI metadata, not scattered shell string building.
- [ ] Registry permissions and login configuration are explicit.
- [ ] Base image selection avoids `alpine` and `slim` variants unless the requirement explicitly calls for them.

## References

- `references/compose-healthchecks.md`
- `references/compose-dependencies.md`
- `references/multiarch-publish.md`
- `references/github-actions-multiarch-template.md`
