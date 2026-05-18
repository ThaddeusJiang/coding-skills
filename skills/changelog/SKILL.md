---
name: changelog
description: Maintain CHANGELOG entries following Keep a Changelog 1.1.0 with project-aligned versioning. Use when creating, updating, or reviewing changelog content for releases.
license: MIT
metadata:
  author: tj
  version: "1.0.0"
---

# Changelog Skill

This skill helps maintain release notes in a consistent, reviewable format based on Keep a Changelog 1.1.0.

## When to Use

Use this skill when tasks include:

- Creating a new `CHANGELOG.md`
- Adding release entries for a version
- Reviewing changelog quality before a release
- Normalizing existing changelog structure

## Version Rule (Project Convention)

- Use SemVer `X.Y.Z` format in headings, for example `## [1.2.0] - 2026-05-18`
- Do not add `v` prefix in changelog version headings
- Keep version value consistent with project release metadata when available

## How to Apply

1. Follow the checklist and template in `references/changelog.md`.
2. Keep sections user-facing and grouped by change type.
3. Ensure each release has an ISO date (`YYYY-MM-DD`).
4. Keep `Unreleased` at the top for upcoming work.

## References

- Main guide: `references/changelog.md`
- Spec: `https://keepachangelog.com/en/1.1.0/`
