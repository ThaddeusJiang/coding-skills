---
name: adr
description: Create, update, and review Architecture Decision Records using the MADR minimal template.
license: MIT
metadata:
  author: amami
  version: "1.1.0"
---

# ADR

Use this skill when the user asks to create, update, supersede, or review an Architecture Decision Record.

This skill intentionally stays minimal. It follows the MADR minimal template and avoids adding repository-specific process unless the repository already has one.

Source template:

- `templates/adr-template-minimal.md`

Reference:

- MADR minimal template: `https://github.com/adr/madr/blob/develop/template/adr-template-minimal.md`

## When to Use

Use this skill for tasks such as:

- Creating a new ADR
- Revising an existing ADR
- Superseding an older ADR with a new one
- Reviewing ADR quality and completeness

## Default Conventions

- Prefer `docs/adr/` when the repository does not already use another ADR directory.
- Use numbered, append-only filenames such as `0001-use-postgres.md`.
- Keep historical ADRs. Do not delete or rewrite decision history.

## How to Apply

1. Find the ADR directory already used by the repository. If none exists, use `docs/adr/`.
2. Read related ADRs before writing a new one or changing direction.
3. Use the minimal template in `templates/adr-template-minimal.md`.
4. Keep the document short and concrete.
5. If a prior ADR is replaced, create a new ADR and add a short note or link in the old ADR instead of rewriting history.

## Writing Rules

- Keep the title specific and solution-oriented.
- Write the context in two or three short paragraphs or an equally compact problem statement.
- List realistic considered options.
- State the chosen option and why it won.
- Record consequences, including downsides.
- Link to issues, RFCs, or related ADRs only when they materially help.

## Review Checklist

- Is the problem statement clear?
- Are the options real alternatives?
- Does the decision outcome explain why this option was chosen?
- Are negative consequences stated explicitly?
- Does the ADR stay short instead of turning into a design doc?

## Guardrails

- Do not silently conflict with accepted ADRs.
- Do not turn ADRs into implementation runbooks.
- Do not add extra mandatory sections unless the repository already requires them.

## Output Expectations

When using this skill, report:

- ADR files created or updated
- Whether a prior ADR was superseded
- Any follow-up docs or code that should align with the decision
