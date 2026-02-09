# Assistant Rules

Shared coding-assistant rules and docs for the Vivian workspace.

## Structure

- `rules/`: Reusable rule files (`*.mdc`) for assistant tooling.
- `docs/`: Reference docs related to assistant workflows.

## Adoption In Sub-Repos

Each git sub-repo should keep a small local `AGENTS.md` containing:

- repo-specific instructions only
- a pointer to this shared repo for common/global rules

This keeps shared guidance centralized while preserving local context.
