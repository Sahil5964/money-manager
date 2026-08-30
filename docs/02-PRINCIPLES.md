---
id: product-principles
name: Product Principles
status: active
audience: [human, agent]
authority: stable
last_updated: 2026-08-30
---

# Product Principles

These principles are the guardrails for product and architecture decisions.

## P1. Nothing is unnecessarily mandatory

A basic money event must remain useful with minimal information.

Do not introduce mandatory data collection merely because richer analysis is possible.

## P2. Enrichment is additive

Adding knowledge should increase understanding without invalidating the simpler record.

A transaction that only knows `amount + date` remains valid after merchant, item, people, or consumption information is added.

## P3. Separate money movement from meaning

`Money left an account` is not automatically equivalent to `personal expense`.

Always preserve the distinction between payment, allocation, responsibility, benefit, and consumption where needed.

## P4. One event may have multiple simultaneous meanings

A single payment may simultaneously be:

- a household purchase,
- partially personal consumption,
- partially for another person,
- and partially a receivable.

Do not force a mutually exclusive classification when reality is compositional.

## P5. Facts, defaults, inferences, estimates, and unknowns are different

Never silently promote an inference into a fact.

The system should preserve enough provenance to explain why a value exists.

## P6. Prefer defaults over repeated questions

The product should learn stable user preferences and patterns.

Defaults reduce effort but remain overridable at more specific scopes.

## P7. Specific overrides beat broad defaults

A global or historical default should never prevent a transaction-specific correction.

Conceptually:

`global default < category default < merchant default < product default < contextual default < explicit transaction override`

The actual precedence may evolve, but specificity must be representable.

## P8. Ask only when uncertainty matters

The system should not ask the user to resolve every ambiguity.

Ask when:

- confidence is low,
- the answer materially changes a useful calculation,
- the user has signaled that the distinction matters,
- or the system cannot safely proceed without clarification.

## P9. AI proposes; deterministic systems calculate and persist truth

LLMs can extract, infer, summarize, plan queries, and explain results.

They should not be the authoritative calculator or source of financial truth when deterministic computation is available.

## P10. Historical events should be explainable

Refunds, returns, reversals, payments, and corrections should preserve history and relationships instead of silently rewriting reality.

## P11. Every new dimension should unlock value

Before adding a field/module, ask:

> **What new questions does this enable?**

If a field cannot support meaningful questions, it may not belong in the product core.

## P12. The model must support messiness without special-case hacks

A new real-world scenario should generally be representable through existing concepts and relationships.

Repeated special-case logic is evidence that the domain model is too narrow.

## P13. Product richness must not equal UI complexity

A very rich underlying model should still support a minimal user experience.

The complexity should live in the system, not be pushed onto the user.

## P14. Learn from correction

When a user corrects an inference, the system should treat the correction as potentially valuable knowledge for future inference, subject to appropriate privacy and control.

## P15. Preserve uncertainty when necessary

Unknown is a valid state.

Do not fabricate precise values because the data model expects one.

## P16. Build the core so optional modules remain optional

A user must not need product-level tracking to use category-level tracking.

A user must not need people tracking to use basic budgeting.

Modules should enhance the same underlying model rather than fragment the product into separate modes.
