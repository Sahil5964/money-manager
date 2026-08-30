---
id: decision-records
name: Product and Architecture Decisions
status: active
audience: [human, agent]
authority: decisions
last_updated: 2026-08-30
---

# Product and Architecture Decisions

This is a lightweight decision record. Add a dated entry when a choice materially affects the product/domain/architecture.

## ADR-001 — Use progressive enrichment instead of mandatory transaction schemas

**Date:** 2026-08-30  
**Status:** accepted

### Decision

The product must support a minimal financial event and allow optional enrichment over time.

### Why

A home/personal money manager should be usable without requiring the user to identify every merchant, person, product, ownership split, or context.

### Consequence

The underlying domain must support partial knowledge naturally.

---

## ADR-002 — Do not model every payment as a simple expense

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Separate money movement from economic responsibility, beneficiary, consumption, and obligation.

### Why

One payment may contain money for several people, shared household spending, gifts, reimbursements, or assets.

### Consequence

“Expense” becomes a derived interpretation in many cases rather than the primitive event type.

---

## ADR-003 — People have roles, not a single ownership field

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Represent people through roles such as payer, beneficiary, owner/economic responsibility holder, consumer, debtor, and creditor.

### Why

These roles frequently differ in real life.

### Consequence

Avoid a one-dimensional `owner_id` style abstraction as the universal relationship.

---

## ADR-004 — Shared allocations do not have to be percentages

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Allow exact amount, percentage, shared pool, household, primary beneficiary, and unknown styles of attribution.

### Why

Many real expenses have shared benefit without meaningful proportional ownership.

---

## ADR-005 — Facts and inferences must be distinct

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Store provenance and uncertainty for inferred/extracted knowledge and keep explicit user assertions distinguishable.

### Why

Financial trust depends on being able to tell what actually came from evidence versus model reasoning.

---

## ADR-006 — AI is not the financial source of truth

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Use AI for extraction, inference, query planning, reasoning, and explanation. Use deterministic application logic for persistence, arithmetic, validation, and financial state transitions.

### Why

LLMs are probabilistic and can hallucinate or miscalculate.

---

## ADR-007 — Model provider independence

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Introduce an abstraction/model-router layer so AI providers can be benchmarked and replaced.

### Why

The best model for extraction may not be the best model for reasoning, cost, or latency over the life of the project.

---

## ADR-008 — Real-world scenarios are domain tests

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Maintain a growing scenario corpus and use it to evaluate domain changes.

### Why

The primary architectural risk is collapsing messy financial reality into overly simple transaction assumptions.

---

## ADR-009 — Every added dimension must justify itself through questions

**Date:** 2026-08-30  
**Status:** accepted

### Decision

New fields/modules should document the useful questions or decisions they enable.

### Why

Prevents feature-rich but low-value complexity.

---

## ADR-010 — Preserve historical truth through related events

**Date:** 2026-08-30  
**Status:** accepted

### Decision

Refunds, returns, reversals, reimbursements, and similar changes should preferably be represented as related events rather than silently rewriting history.

### Why

The system should explain how a final state emerged from real-world events.

---

## Decision template for future entries

```markdown
## ADR-XXX — <title>

**Date:** YYYY-MM-DD
**Status:** proposed | accepted | superseded | rejected

### Decision

### Context / problem

### Alternatives considered

### Why this choice

### Consequences

### Related scenarios

### Related questions
```
