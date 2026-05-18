# Changelog Checklist (Keep a Changelog 1.1.0)

## File Header

- Title uses `# Changelog`
- Include short intro text:
  - "All notable changes to this project will be documented in this file."
  - "The format is based on Keep a Changelog, and this project adheres to Semantic Versioning."

## Required Structure

- Keep one top-level `## [Unreleased]` section at the top
- For each release, use:
  - `## [X.Y.Z] - YYYY-MM-DD`
- Use standard subsections when needed:
  - `### Added`
  - `### Changed`
  - `### Deprecated`
  - `### Removed`
  - `### Fixed`
  - `### Security`

## Versioning Rule (Project Convention)

- Version heading must be SemVer `X.Y.Z`
- Do not use `vX.Y.Z` in changelog headings
- Keep headings and release metadata aligned with actual release version

## Quality Checklist

- Entries describe user-visible outcomes, not implementation trivia
- Wording is concise and testable
- No duplicated entries across versions
- Date is accurate and in ISO format
- `Unreleased` is retained after cutting a release

## Minimal Template

```md
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- 

## [1.0.0] - 2026-05-18

### Added
- Initial release.
```
