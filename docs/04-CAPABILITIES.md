---
id: capability-map
name: Capability Map
status: active
audience: [human, agent]
authority: evolving
last_updated: 2026-08-30
---

# Capability Map

## How to use this document

Capabilities are grouped by domain, not by release. A capability being documented does not mean it must exist now.

Suggested status values:

- `core`: fundamental to the product identity
- `optional`: enriches the experience but must not be required
- `future`: part of the long-term direction
- `experimental`: worth exploring but not committed

## 1. Money Core

### Accounts — core

Track bank accounts, cash, cards, and other financial containers.

### Transactions / Financial Events — core

Record imported or manually entered money events.

### Transfers — core

Represent movement between accounts without misclassifying it as spending.

### Reconciliation — optional

Match imported events, statements, and recorded events.

### Multi-currency — future

Preserve original and settlement currency details.

## 2. Classification and Understanding

### Merchant normalization — core/optional

Map noisy merchant descriptions to canonical counterparties.

### Category classification — core

Assign useful categories without excessive user work.

### Recurring pattern detection — optional

Identify recurring charges and commitments.

### Anomaly detection — future

Detect unusual merchants, amounts, timing, or patterns.

## 3. People and Shared Money

### People — optional

Represent family, friends, colleagues, household members, etc.

### Beneficiary relationships — optional

Track who benefited from an event.

### Shared allocations — optional

Support exact amounts, percentages, shared pools, or other allocation modes.

### Reimbursements / receivables — optional

Track money expected back.

### Gifts / support — optional

Distinguish money given intentionally from money expected back.

### Debts / loans — future

Support explicit lending/borrowing lifecycle.

## 4. Product and Consumption Intelligence

### Receipt / invoice parsing — optional

Extract item-level information from receipts, emails, PDFs, and images.

### Product normalization — future

Map product names to stable product identities.

### Brand / variant — future

Preserve brand and variant information where available.

### Quantity and units — optional/future

Support grams, liters, units, packs, subscriptions, etc.

### Consumption tracking — future

Separate purchased quantity from actual consumption.

### Price history — future

Track product-level price changes over time.

## 5. Context and Intent

### Trips — optional

Group events into a trip without replacing normal categories.

### Projects / occasions — optional

Examples: wedding, moving, renovation, birthday, festival.

### Intent — future

Capture why something was purchased or paid.

### Location — future

Use coarse useful context such as city, travel, home, office, or online.

## 6. Assets and Commitments

### Asset awareness — optional/future

Distinguish acquisition of durable value from consumption.

### Subscription tracking — optional

Model recurring services as commitments.

### Prepayments / advances — future

Represent money paid before the final expense is known.

### Tax / fee components — future

Preserve tax, tip, service fee, cashback, discounts, withholding, etc.

## 7. AI Intelligence

### Structured extraction — core for AI-enriched paths

Convert messy sources into candidate structured facts.

### Personal defaults — optional → highly valuable

Learn repeated user preferences and assumptions.

### Ambiguity resolution — optional

Combine sources and history to propose interpretations.

### Natural-language queries — future/experimental

Let users ask complex questions conversationally.

### Financial analysis — future

Explain trends and changes using deterministic calculations + AI reasoning.

### Forecasting — future

Predict cash flow, commitments, and selected consumption patterns.

## Modularization rule

Every capability should answer:

1. What does it know that the simpler system did not?
2. What new questions does that knowledge unlock?
3. How can it be used without forcing the rest of the product to exist?

## Example of progressive value

```text
Amount/date
  → "How much did I spend?"

+ merchant
  → "How much did I spend at Blinkit?"

+ category
  → "How much did I spend on groceries?"

+ item
  → "How much paneer did I buy?"

+ brand
  → "Which paneer brand do I buy most?"

+ person
  → "How much did I spend for my brother?"

+ obligation
  → "How much does my brother owe me?"

+ consumption
  → "How much paneer do I actually consume per month?"

+ context
  → "Why was grocery spending higher during the trip?"
```
