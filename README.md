# Money Manager — Product Blueprint

**Status:** Living product blueprint  
**Audience:** Human collaborators and coding/AI agents (Claude Code, Codex, etc.)  
**Canonical docs:** `docs/`  
**Last updated:** 2026-08-30

## What this project is

A progressively intelligent money-management system that starts as a simple money tracker and can grow into a model of a person's financial life.

The core product idea is:

> A financial event should remain useful with very little information. Every additional fact, inference, relationship, or context should increase what the system can answer — not make the event harder to use.

The system therefore treats money events as **progressively enrichable** rather than as rigid forms with mandatory fields.

## Documentation order

Read `docs/00-HOW-TO-READ.md` first. It explains the role, priority, and intended reading order of every document.

Recommended human reading order:

1. `01-VISION.md`
2. `02-PRINCIPLES.md`
3. `03-DOMAIN.md`
4. `04-CAPABILITIES.md`
5. `05-SCENARIOS.md`
6. `06-QUESTIONS.md`
7. `07-AI.md`
8. `08-DATA-TRUTH.md`
9. `09-DECISIONS.md`
10. `10-ROADMAP.md`
11. `12-VERSION-PLAN.md`

Recommended agent reading order is defined explicitly in `00-HOW-TO-READ.md`.

## Non-goals of this documentation

This is not a full SRS, API specification, database migration plan, or implementation contract.

It intentionally separates:

- product intent,
- domain understanding,
- desired capabilities,
- real-world examples,
- AI behavior,
- trust/uncertainty rules,
- architectural decisions,
- and current delivery priorities.

Implementation details belong in the codebase and technical design documents when they become necessary.

## Canonical terminology

Use the terms in `03-DOMAIN.md` consistently. In particular, avoid using `transaction`, `expense`, `owner`, or `split` as catch-all concepts when a more precise domain term exists.

## Delivery plan

Use `docs/12-VERSION-PLAN.md` when deciding what a version should contain. It is intentionally self-contained per version: goal, user promise, scope, architecture, AI role, quality bar, exit criteria, and non-goals.

## Agent rule

When code conflicts with the product blueprint, do not silently reinterpret the blueprint. Identify the conflict and prefer the latest explicit architectural decision or ask for human direction when the conflict materially changes the product model.
