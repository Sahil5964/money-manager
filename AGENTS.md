# Agent Context Contract

This repository contains the product blueprint for the Money Manager project.

## Before changing code

Read:

1. `docs/00-HOW-TO-READ.md`
2. `docs/01-VISION.md`
3. `docs/02-PRINCIPLES.md`
4. `docs/03-DOMAIN.md`
5. `docs/08-DATA-TRUTH.md`
6. `docs/09-DECISIONS.md`

Then load only the capability/scenario/AI/roadmap documents relevant to the task.

## Core invariants

1. Financial events may be partially known.
2. Enrichment is optional and additive.
3. Money movement is not the same as expense.
4. Payer, payee, economic responsibility, beneficiary, consumer, debtor, and creditor are distinct roles.
5. Shared allocation does not always mean percentage splitting.
6. Facts, user assertions, defaults, inferences, estimates, and unknowns must remain distinguishable.
7. AI is not the financial source of truth.
8. Deterministic systems should perform authoritative calculations and state transitions.
9. Historical events should remain explainable; prefer related events for refunds/reversals/corrections.
10. New complexity should justify itself through useful questions or user value.

## When implementing a feature

Ask:

- Which domain concept(s) does this use?
- Is the feature optional or accidentally becoming mandatory?
- What new questions does it enable?
- What happens when the information is unknown?
- Is any value inferred? If so, where is provenance/confidence represented?
- Could a real-world scenario break this model?
- Does this require an architectural decision record?
- Which version should contain this work, and does that version's architecture remain coherent?

## When working with AI

Do not:

- hallucinate missing transaction details,
- replace authoritative data with an LLM output,
- make the model responsible for arithmetic that code can do exactly,
- collapse inference into fact,
- or add a chat UI without a trustworthy domain/query layer underneath.

## When discovering new reality

Update the documentation rather than hiding the discovery in code:

- new domain concept → `03-DOMAIN.md`
- new messy case → `05-SCENARIOS.md`
- new question → `06-QUESTIONS.md`
- new AI behavior → `07-AI.md`
- new trust/provenance rule → `08-DATA-TRUTH.md`
- durable design choice → `09-DECISIONS.md`
- changed delivery priority → `10-ROADMAP.md`

## If documentation and code disagree

Do not silently invent a compromise. Check recent decisions and identify the conflict. Preserve the product intent unless a newer explicit decision supersedes it.
