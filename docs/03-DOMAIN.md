---
id: financial-domain-model
name: Financial Domain Model
status: active
audience: [human, agent]
authority: domain
last_updated: 2026-08-30
---

# Financial Domain Model

## Purpose

This document defines the conceptual model. It is not a database schema.

A concept may later be implemented as an entity, event, relationship, observation, rule, or derived view.

## Core abstraction: Financial Event

A **Financial Event** is something that happened or is believed to have happened in a person's financial world.

Examples:

- purchase,
- transfer,
- payment,
- refund,
- withdrawal,
- deposit,
- loan,
- gift,
- reimbursement,
- investment contribution,
- asset acquisition,
- subscription renewal.

A financial event may have many optional dimensions.

## Money Movement

Represents movement of money or a monetary claim.

Potential attributes:

- amount,
- currency,
- source account,
- destination account,
- payer,
- payee,
- payment method,
- fees,
- exchange rate,
- settlement status,
- timestamp.

A money movement is not automatically a personal expense.

## Transaction

Use **transaction** as a practical term for a recorded financial event or money movement when the context is unambiguous.

Avoid treating transaction as the complete semantic model.

## Account

A container or source of money/financial position.

Examples:

- bank account,
- cash wallet,
- credit card account,
- brokerage account,
- investment account.

## Merchant / Counterparty

The organization or person involved in a financial event.

Examples:

- Blinkit,
- Amazon,
- a landlord,
- a friend,
- a bank.

Merchant identity may require normalization across noisy source descriptions.

## Product

A tangible or digital item that may be purchased, owned, used, or consumed.

Potential attributes:

- canonical name,
- brand,
- model/variant,
- quantity,
- unit,
- SKU/identifier,
- price,
- discount allocation,
- tax allocation.

Product tracking is optional.

## Service

A non-product good or recurring service.

Examples:

- Netflix subscription,
- internet service,
- hotel stay,
- taxi ride.

## Person

A human who can play one or more roles in relation to a financial event.

A person may be:

- payer,
- payee,
- beneficiary,
- owner,
- consumer,
- debtor,
- creditor,
- recipient of a gift,
- member of a household.

These roles must not be collapsed into a single `person_type`.

## Allocation

A way of attributing part of an event to a person, group, purpose, product, context, or financial responsibility.

Allocation may be expressed using:

- exact amount,
- percentage,
- equal/weighted split,
- shared/common pool,
- primary beneficiary,
- household,
- unknown.

Not every shared event needs per-person percentages.

## Economic Responsibility / Ownership

Represents who is economically responsible for the cost or who should ultimately bear it.

This can differ from who paid.

Example:

A user pays ₹5,000 for a company expense. The user is the payer, but the company may be the economic responsibility holder.

## Beneficiary

Who benefits from the purchase or payment.

Beneficiary is not necessarily the payer, owner, or consumer.

## Consumer

Who actually uses or consumes the purchased item/service.

A household grocery purchase may benefit multiple people but have no meaningful exact ownership split.

## Obligation

A relationship representing money or value that is expected to move between parties.

Examples:

- receivable,
- payable,
- reimbursement,
- loan,
- advance,
- deposit,
- security deposit,
- credit.

An obligation can exist without being a conventional expense.

## Asset

A thing/value acquired that may remain owned rather than immediately consumed.

Examples:

- laptop,
- phone,
- furniture,
- vehicle,
- financial investment.

Do not equate cash outflow with expense in all cases.

## Consumption

Represents use or depletion of a product/service/value.

Consumption may differ from purchase date and purchase quantity.

Example:

Annual subscription paid in one day can represent 12 months of future consumption.

## Context

A real-world grouping that cuts across normal categories.

Examples:

- trip,
- wedding,
- birthday,
- moving house,
- project,
- festival,
- family visit.

Context answers questions that categories cannot.

## Intent

Why the user or another actor made the purchase/payment.

Examples:

- personal use,
- gift,
- family support,
- business expense,
- travel,
- celebration.

Intent is distinct from merchant and category.

## Commitment / Recurrence

A recurring or future expectation connected to financial events.

Examples:

- rent,
- EMI,
- subscription,
- insurance,
- SIP,
- school fee.

A recurring series should not be represented merely as unrelated transactions.

## Lifecycle

A financial event or purchase can move through states such as:

`initiated → pending → completed → returned/reversed/refunded`

Exact states depend on the event type.

## Evidence

A source from which a fact or candidate fact is derived.

Examples:

- bank feed,
- SMS,
- email receipt,
- PDF invoice,
- image receipt,
- user input,
- imported file,
- merchant order record,
- previous user behavior.

## Fact

Information considered authoritative for the current model.

Examples:

> Bank record says ₹2,846.

> User explicitly said this purchase was for brother.

## Inference

A reasoned conclusion that is not directly authoritative.

Example:

> Protein powder was probably purchased for brother based on historical behavior.

## Default

A reusable assumption that can be applied when no more specific information exists.

A default must remain overridable.

## Estimate

A value intentionally represented as approximate rather than exact.

Example:

> Estimated monthly consumption = 2.4kg.

## Unknown

A valid representation of insufficient knowledge.

Unknown must not automatically trigger invented values.

## Confidence

A measure associated with inferred or extracted information when appropriate.

Confidence is not the same as truth. A high-confidence inference can still be wrong.

## Provenance

Information about where a value came from and how it was produced.

At minimum, useful provenance can describe:

- source,
- extraction/inference method,
- timestamp,
- confidence when applicable,
- user correction when applicable.

## Relationships between events

Events should be able to relate to other events.

Examples:

- purchase → return,
- purchase → refund,
- credit-card purchase → bill payment,
- advance → final expense,
- payment → reimbursement,
- order → delivery,
- investment contribution → portfolio position.

## Example: messy Blinkit purchase

```text
Financial Event: Blinkit order, ₹2,846
    |
    +-- Money movement
    |     +-- payer: user
    |     +-- source: credit card
    |
    +-- Merchant
    |     +-- Blinkit
    |
    +-- Items
    |     +-- Paneer, 400g, Brand A, ₹320
    |     +-- Milk, 2L, Brand B, ₹240
    |     +-- Protein powder, ₹900
    |     +-- ...
    |
    +-- Beneficiaries / consumers
    |     +-- user: some items
    |     +-- household: some items
    |     +-- brother: protein powder
    |
    +-- Obligation
    |     +-- brother reimbursement: unknown / optional
    |
    +-- Evidence
    |     +-- bank transaction
    |     +-- order invoice
    |
    +-- Inference
          +-- brother association derived from prior behavior
```

The model must allow this richness without requiring all of it.

## Key domain rule

> **Do not model the world as one transaction row with many mandatory attributes. Model an event and the facts/relationships known about that event.**
