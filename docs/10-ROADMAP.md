---
id: product-roadmap
name: Product Roadmap
status: active
audience: [human, agent]
authority: delivery
last_updated: 2026-08-30
---

# Product Roadmap

This roadmap intentionally separates the long-term product vision from what should be built immediately.

## Phase 0 — Domain foundation

**Goal:** prove that the conceptual model survives messy examples.

Focus:

- Financial Event model
- Money Movement
- Account
- Merchant
- Category
- Person/role relationships
- Allocation
- Obligation
- Evidence/provenance
- Fact/default/inference distinction
- Scenario test corpus

Exit signal:

The team can model the initial scenario set without special-case fields that undermine the core model.

## Phase 1 — Simple money manager

**Goal:** be genuinely useful to anyone with minimal effort.

Focus:

- accounts,
- transactions,
- transfers,
- categories,
- merchant recognition,
- basic analytics,
- import/reconciliation path.

User experience requirement:

A basic user can use the product without knowing anything about people, products, or AI.

## Phase 2 — Personal defaults and shared money

**Goal:** reduce manual attribution and support real-world relationships.

Focus:

- people,
- beneficiary relationships,
- shared allocations,
- gifts/support,
- reimbursements,
- learned defaults,
- lightweight review UI.

Key success signal:

The system asks fewer questions over time as it learns patterns.

## Phase 3 — Document and product intelligence

**Goal:** move beyond merchant/category into item-level understanding.

Focus:

- receipt/invoice parsing,
- PDF/image/email ingestion,
- product extraction,
- brand/variant normalization,
- quantity/units,
- provenance and confidence review.

## Phase 4 — Consumption intelligence

**Goal:** distinguish purchasing from actual consumption/use.

Focus:

- consumption allocations,
- product usage history,
- purchase vs consumption timelines,
- price-per-unit trends,
- household vs personal consumption.

## Phase 5 — Financial intelligence

**Goal:** answer complex questions and explain change.

Focus:

- natural-language queries,
- financial graph querying,
- trend explanations,
- anomaly detection,
- forecasting,
- contextual analysis.

## Phase 6 — Long-term vision

Potential directions:

- predictive financial planning,
- richer asset tracking,
- deeper commitments,
- tax-aware analysis,
- proactive assistance,
- what-if simulation,
- stronger personalized financial intelligence.

These are intentionally not commitments.

## Prioritization rule

A capability should move forward when it provides substantial new user value without forcing unrelated complexity.

## Roadmap anti-patterns

Avoid:

- implementing every concept in the domain at once,
- turning future capability into current mandatory workflow,
- building AI chat before the underlying financial model is trustworthy,
- creating dozens of settings before defaults/learning have been validated,
- optimizing for field completeness instead of usefulness.


## Version detail

For the self-contained implementation plan behind these phases, see `12-VERSION-PLAN.md`. The version plan is the detailed delivery guide; this roadmap remains the compact priority map.
