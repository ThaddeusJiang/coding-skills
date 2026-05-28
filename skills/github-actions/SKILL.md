---
name: github-actions
description: Enforce secure and reproducible GitHub Actions workflows by pinning every action to an immutable commit SHA with an explicit version comment.
license: MIT
metadata:
  author: amami
  version: "1.0.0"
---

# GitHub Actions Skill

Create and review GitHub Actions workflows with strict action pinning rules.

## When to Use

Use this skill when tasks include:

- Creating or updating `.github/workflows/*.yml`
- Reviewing workflow supply-chain security
- Standardizing action usage across repositories

## Hard Rule

- Every `uses:` reference in workflows must pin to a full commit SHA.
- Keep the readable action version in an inline comment.
- Required format:
  - `actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1`

## How to Apply

1. Find all `uses:` entries in workflow files.
2. Replace tag-based references (such as `@v4`) with full commit SHAs.
3. Add or keep inline version comments like `# vX.Y.Z`.
4. Verify no workflow action remains tag-only or branch-based.

## Acceptance Checklist

- [ ] All workflow `uses:` entries are pinned to full commit SHAs.
- [ ] Every pinned action has a version comment in the same line.
- [ ] No `@main`, `@master`, or floating tag-only action references remain.

## Troubleshooting: Trigger PR Workflows

When a Pull Request exists but no `pull_request` workflow run appears, push an empty commit to emit a `synchronize` event:

```bash
git checkout <pr-branch>
git commit --allow-empty -m "chore: trigger GitHub Actions for PR #<number>"
git push origin <pr-branch>
```

Then verify checks:

```bash
gh pr checks <pr-number> --repo <owner>/<repo>
```
