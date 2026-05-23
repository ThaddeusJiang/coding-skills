---
name: "worktree"
description: "Prepare any git worktree by copying shared files from the main checkout using git common-dir metadata."
---

# Worktree Prepare Skill

Prepare a worktree by copying shared local files from the main checkout.

## Inputs

- A list of file paths that should be copied from main checkout to current worktree.
- Default files when no list is provided:
  - `.env`
  - `AGENTS.override.md`

## Steps

1. Resolve the main checkout root from current worktree:
   - `MAIN_ROOT="$(cd "$(git rev-parse --git-common-dir)/.." && pwd)"`
2. Build file list:
   - Use caller-provided files, or fallback to default files.
3. For each file in list:
   - Check source exists: `test -f "$MAIN_ROOT/$file"`
   - If missing, stop and report the missing source file.
   - Ensure destination parent exists: `mkdir -p "$(dirname "$file")"`
   - Copy file: `cp "$MAIN_ROOT/$file" "$file"`

## Notes

- Run inside a git worktree root.
- This skill is repository-agnostic and should not hardcode project-specific paths.
