# How to Read the Product Blueprint

## Purpose

This file is the entry point for humans and AI coding agents. It explains what each artifact means, how authoritative it is, and how to use the set without treating it like a traditional SRS.

## One-sentence mental model

The docs describe **why the product exists, the model of financial reality it uses, the questions it should answer, the capabilities that can emerge from that model, and the constraints that keep implementation honest.**

## Reading order for a human collaborator

### 1. `01-VISION.md`
Read this to understand the product's north star.

### 2. `02-PRINCIPLES.md`
Read this to understand the product rules that should survive feature changes.

### 3. `03-DOMAIN.md`
Read this to understand what the application believes a financial event and related concepts are.

### 4. `04-CAPABILITIES.md`
Read this to see the feature/capability space without confusing the entire vision with the current release.

### 5. `05-SCENARIOS.md`
Read these concrete messy cases to see whether the model survives real life.

### 6. `06-QUESTIONS.md`
Read this to understand the outcomes the product is trying to make possible.

### 7. `07-AI.md`
Read this to understand what AI is responsible for and what must remain deterministic.

### 8. `08-DATA-TRUTH.md`
Read this before implementing anything that stores or presents inferred information.

### 9. `09-DECISIONS.md`
Read this to understand important choices already made and why.

### 10. `10-ROADMAP.md`
11. `12-VERSION-PLAN.md`
Read this last. It tells you what is actually being built now versus what is merely envisioned.

## Reading order for AI agents

When an agent is starting a task, load the minimum set needed, then expand only when needed.

### Always load

- `01-VISION.md`
- `02-PRINCIPLES.md`
- `03-DOMAIN.md`
- `08-DATA-TRUTH.md`
- `09-DECISIONS.md`

### Load for product/feature work

- `04-CAPABILITIES.md`
- `05-SCENARIOS.md`
- `06-QUESTIONS.md`
- `10-ROADMAP.md`

### Load for version/release planning

- `10-ROADMAP.md`
- `12-VERSION-PLAN.md`
- the domain/capability/scenario documents relevant to the target version

`12-VERSION-PLAN.md` is the bridge between the stable product model and concrete release planning. It should be read when deciding what belongs in a version and what architecture is justified at that stage.

### Load for AI work

- `07-AI.md`
- `08-DATA-TRUTH.md`
- relevant sections of `03-DOMAIN.md`
- relevant scenarios involving ambiguity and inference

## Document authority

Highest authority to lowest:

1. Explicit current user/product decision.
2. `09-DECISIONS.md` for recorded architecture/product decisions.
3. `01-VISION.md` and `02-PRINCIPLES.md` for stable product intent.
4. `03-DOMAIN.md` for canonical terminology and conceptual relationships.
5. `08-DATA-TRUTH.md` for evidence/inference semantics.
6. `04-CAPABILITIES.md`, `05-SCENARIOS.md`, `06-QUESTIONS.md` for evolving capability discovery.
7. `10-ROADMAP.md` for current implementation priority.
8. Code and implementation details, unless explicitly designated as the current decision.

If two documents appear to disagree, do not merge them mentally. Treat the disagreement as a documentation defect and update the relevant decision record.

## How to extend this documentation

When discovering something new:

- Add a durable product rule to `02-PRINCIPLES.md` only if it should govern many future decisions.
- Add a domain concept or relationship to `03-DOMAIN.md` if it changes the model of reality.
- Add a real-world case to `05-SCENARIOS.md` when it exposes a new type of messiness.
- Add a desired user question to `06-QUESTIONS.md`.
- Add or modify AI responsibilities in `07-AI.md`.
- Add provenance/confidence semantics to `08-DATA-TRUTH.md`.
- Record an important choice in `09-DECISIONS.md`.
- Change delivery priority in `10-ROADMAP.md`.

## Suggested metadata for future documents

Every new canonical document should begin with:

```yaml
---
id: unique-kebab-case-id
name: Human-readable name
status: draft | active | superseded | archived
audience: [human, agent]
authority: stable | domain | evolving | delivery
last_updated: YYYY-MM-DD
---
```

## Important distinction

These docs describe the **meaning of the product**, not every possible implementation.

An agent should not create a database column simply because a concept exists here. A concept may be represented by a normalized entity, relationship, event, observation, inference, rule, or derived view.
