# Agent Task Template

Use this template when asking Claude Code, Codex, or another coding agent to make changes in the repository.

## Task

Describe the user-visible outcome.

## Context

Relevant docs:

- `docs/01-VISION.md`
- `docs/02-PRINCIPLES.md`
- `docs/03-DOMAIN.md`
- `docs/08-DATA-TRUTH.md`
- relevant scenario(s): `SXX`, `SYY`

## Constraints

- Do not introduce mandatory fields unless explicitly justified.
- Preserve facts vs inference vs default semantics.
- Prefer deterministic calculation for financial arithmetic.
- Preserve historical relationships where applicable.
- Do not create special-case logic that violates the domain model.

## Acceptance criteria

Write observable outcomes.

## Documentation updates

List any blueprint documents that should be updated if the implementation changes a product/domain decision.
