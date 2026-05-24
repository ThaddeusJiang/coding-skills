# coding-skills

[![skills.sh](https://skills.sh/b/ThaddeusJiang/coding-skills)](https://skills.sh/ThaddeusJiang/coding-skills)

Reusable coding skills by ThaddeusJiang for Codex, Claude Code, Cursor, and other agent environments supported by the `skills` CLI.

## Install

Install the full repository:

```bash
npx skills add ThaddeusJiang/coding-skills
```

List available skills before installing:

```bash
npx skills add ThaddeusJiang/coding-skills --list
```

Install a single skill:

```bash
npx skills add ThaddeusJiang/coding-skills --skill changelog
```

## Available Skills

- [changelog](skills/changelog/)
  - Maintain release notes using Keep a Changelog 1.1.0 with SemVer `X.Y.Z` headings (no `v` prefix).
- [docker-best-practices](skills/docker-best-practices/)
  - Docker and Docker Compose best practices for reliable local/dev deployments and multi-arch image publishing.
- [frontend-coding-guidelines](skills/frontend-coding-guidelines/)
  - Frontend coding conventions and review checklist for React/TypeScript projects.
- [worktree](skills/worktree/)
  - Prepare any git worktree by copying shared files from the main checkout using git common-dir metadata.

## Usage Notes

- Skills are discovered automatically from the repository by the `skills` CLI.
- This repository is structured for the open `skills.sh` ecosystem.
- Install counts shown on `skills.sh` are based on anonymous `skills` CLI telemetry.

## More `npx skills` Commands

### Install from this repository

```bash
npx skills add ThaddeusJiang/coding-skills --list
npx skills add ThaddeusJiang/coding-skills --skill changelog
npx skills add ThaddeusJiang/coding-skills --skill docker-best-practices
npx skills add ThaddeusJiang/coding-skills --all
```

### Manage installed skills

| Command | Description |
| --- | --- |
| `npx skills list` | List installed skills (`ls` alias is also supported) |
| `npx skills find [query]` | Search for skills interactively or by keyword |
| `npx skills remove [skills]` | Remove installed skills from agents |
| `npx skills update [skills]` | Update installed skills to the latest versions |
| `npx skills init [name]` | Create a new `SKILL.md` template |

### Common examples

```bash
# List installed skills
npx skills list
npx skills ls -g

# Search for skills
npx skills find
npx skills find docker

# Update installed skills
npx skills update
npx skills update changelog
npx skills update -g

# Remove installed skills
npx skills remove changelog
npx skills remove --all

# Create a new skill template
npx skills init
npx skills init my-new-skill
```

## References

- `skills.sh` directory: https://www.skills.sh/ThaddeusJiang/coding-skills
- `skills` CLI docs: https://www.skills.sh/docs
- `skills` CLI repository: https://github.com/vercel-labs/skills
