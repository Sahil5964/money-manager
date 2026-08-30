---
id: version-plan
name: Version-by-Version Product and Architecture Plan
status: active
audience: [human, agent]
authority: delivery
last_updated: 2026-08-30
---

# Version-by-Version Product and Architecture Plan

## Purpose

This is the project's **self-contained delivery plan**. Each version is a coherent product slice with its own purpose, architecture scope, domain growth, AI role, UX expectations, quality bar, and exit criteria.

This is intentionally more detailed than `10-ROADMAP.md` but less rigid than an SRS. The plan explains **what a version should make possible and why**, while allowing implementation details to evolve.

## Product strategy

The product should grow by **adding dimensions of understanding**, not by making the basic transaction workflow progressively harder.

The version sequence therefore follows this path:

> Money → Trustworthy structure → Personal defaults → People/relationships → Documents/products → Consumption → Intelligence

Every version must preserve the properties of earlier versions.

## Architecture strategy across all versions

### Core architectural stance

Use a **modular monolith first**, with clear module boundaries and explicit contracts. Do not start with microservices.

Recommended conceptual layers:

```text
UI / API
   ↓
Application services
   ↓
Domain modules
   ├── Money
   ├── Accounts
   ├── Events
   ├── Merchants
   ├── People
   ├── Products
   ├── Relationships
   ├── Context
   ├── Consumption
   └── Intelligence
   ↓
Persistence / search / object storage
   ↓
External providers
```

### Recommended baseline stack

The exact stack is not a product decision, but a pragmatic starting point is:

- **Application:** one modular backend application.
- **Primary database:** PostgreSQL.
- **Object storage:** S3-compatible storage for invoices, receipts, images, and raw imported documents.
- **Background jobs:** a simple durable job queue; start inside the application infrastructure rather than introducing Kafka or a distributed workflow system.
- **Search:** PostgreSQL full-text/search first; introduce a dedicated vector/search system only when measured needs justify it.
- **AI gateway:** one internal abstraction over model providers so Gemini, OpenAI, Anthropic, or local models can be changed without rewriting domain code.
- **Analytics:** derived queries/materialized views first; do not build a separate warehouse until necessary.

### Non-negotiable architectural properties

1. Financial truth is deterministic and persisted independently of AI.
2. AI outputs are proposals, enrichments, inferences, or explanations with provenance.
3. Events can be partially known.
4. Enrichment is additive and optional.
5. Related financial events should model refunds, reversals, settlements, and corrections rather than silently mutating history.
6. Every major module should be independently useful and removable from the default UX.
7. Domain modules should communicate through stable domain/application contracts rather than leaking database tables everywhere.
8. The system should preserve raw evidence wherever practical so later AI models can improve extraction without losing the original input.

---

# Version 0 — Foundation / Reality Model

## Goal

Prove that the product's domain model can represent messy real-world money situations without collapsing into a giant `transactions` table or a collection of special cases.

## User promise

> The product may not look impressive yet, but the underlying model can represent the real world we eventually want to understand.

## What this version builds

- Financial Event abstraction.
- Money Movement abstraction.
- Accounts.
- Basic Merchant and Category concepts.
- Person and role relationships.
- Allocation concept.
- Obligation / receivable / payable concept.
- Evidence and provenance model.
- Fact / assertion / default / inference / estimate / unknown semantics.
- Relationship between related events.
- Scenario test corpus.

## What the user can do

This can be mostly developer-facing or a tiny internal interface. The main outcome is model validation, not polished UX.

The team should be able to represent examples such as:

- a purchase for self plus brother;
- a gift to a parent;
- a reimbursement expected but later forgiven;
- a transfer between accounts;
- a purchase followed by partial refund;
- a credit-card purchase followed by bill payment;
- an annual subscription paid upfront;
- an unknown transaction;
- a purchase whose item details are available only in an invoice.

## Architecture

### Components

```text
API / thin admin UI
        ↓
Application layer
        ↓
Domain modules
  Event
  Money
  Account
  People
  Allocation
  Evidence
  Relationship
        ↓
PostgreSQL
        ↓
Object storage (optional at this stage)
```

### Important implementation choice

Use explicit domain objects/relationships even if the first persistence model is simple. Avoid designing around UI forms.

## AI role

Minimal.

AI may be used in experiments to test extraction/inference, but AI should not be on the authoritative write path yet.

## Data model emphasis

The key question is whether concepts can coexist without conflict:

```text
payer
source account
payee / merchant
beneficiary
consumer
responsible party
creditor
debtor
allocation
obligation
context
fact / inference / default
```

## Quality bar

Create a scenario test suite. A scenario fails if it requires inventing a one-off field or changing the meaning of an existing concept just to fit it.

## Exit criteria

- Core scenario corpus is representable.
- Historical events can remain explainable.
- Unknown information is valid state.
- Inference is distinguishable from fact.
- The domain does not require every transaction to have a category/person/product.

## Explicitly not building

- Full bank integrations.
- Product-grade AI assistant.
- Detailed consumption calculations.
- Complex UI.
- Microservices.

---

# Version 1 — Useful Money Manager

## Goal

Create a genuinely useful application for a person who wants **simple money management and does not care about the advanced model**.

## User promise

> Add or import money activity with almost no effort and immediately understand where your money went.

## Product scope

### Core

- Accounts.
- Income and outgoing money events.
- Transfers.
- Categories.
- Merchants.
- Search.
- Basic dashboard.
- Monthly summaries.
- Import/reconciliation path.

### UX principle

A user must be able to use the app while knowing almost nothing about the deeper model.

Example:

```text
₹482
Swiggy
Food
```

is a complete usable record.

## Architecture

```text
Web/mobile UI
     ↓
API
     ↓
Application services
     ↓
Money + Account + Event + Merchant + Category modules
     ↓
PostgreSQL

Optional:
Import pipeline → normalization → validation → event creation
```

## AI role

Small and non-essential.

Use AI only where confidence is high and failure is cheap:

- merchant normalization;
- category suggestion;
- duplicate candidate detection;
- basic transaction description cleanup.

Every AI result should be overrideable.

## Analytics

Answer:

- How much did I spend?
- Where did it go?
- How is this month different from last month?
- What are my biggest categories/merchants?

Do not yet attempt product-level consumption analysis.

## Quality bar

- Basic transaction capture is fast.
- Transfers do not count as spending.
- Refunds/reversals do not silently corrupt historical numbers.
- Importing the same source twice does not create uncontrolled duplicates.
- Core balances and totals are deterministic.

## Exit criteria

A basic user can use the application for several weeks without touching any advanced fields and still get meaningful value.

## Explicitly not building

- People allocation UI.
- Product catalog.
- AI chat.
- Predictive forecasting.
- Advanced relationship accounting.

---

# Version 2 — Personal Defaults and Shared Money

## Goal

Make the application understand that a user's money often benefits other people, without requiring users to manually split everything.

## User promise

> The application starts learning how I normally think about money, people, and shared expenses.

## Product scope

- People.
- Beneficiary relationships.
- Shared allocations.
- Gifts/support.
- Receivables and payables.
- Reimbursements.
- Optional family/household context.
- User-defined defaults.
- Learned defaults.
- Review queue for uncertain enrichments.

## Core modeling rule

Do not collapse these into one field:

```text
payer ≠ economic owner ≠ beneficiary ≠ consumer ≠ debtor ≠ creditor
```

## UX

The user should usually see suggestions rather than forms.

Example:

> ₹900 protein powder — likely Brother (87%)
>
> [Confirm] [Change]

The app should remember confirmed corrections where appropriate.

## Architecture

```text
Money Core
   ↓
People / Relationship module
   ↓
Allocation + Obligation module
   ↓
Rules / Defaults engine
   ↓
Review queue
```

Introduce a lightweight rules engine before building complex machine learning.

## AI role

- infer likely beneficiary/person;
- propose allocation;
- detect recurring patterns;
- learn user vocabulary and defaults;
- prioritize ambiguous cases for review.

AI should produce:

```text
value
confidence
provenance
reason / evidence
```

## New questions enabled

- How much did I spend for my brother?
- How much do people owe me?
- How much did I gift my parents?
- How much of my spending is actually mine?
- Which shared expenses are unusually high?

## Quality bar

- No advanced relationship field becomes mandatory for ordinary transactions.
- User corrections are durable and explainable.
- A mistaken inference can be corrected without rewriting financial history.

## Exit criteria

The system can model common family/friend money situations and begins reducing manual attribution over time.

## Explicitly not building

- Exact consumption tracking.
- Full receipt OCR/vision pipeline.
- Autonomous financial decisions.

---

# Version 3 — Document and Product Intelligence

## Goal

Move from **merchant-level understanding** to **item-level understanding**.

## User promise

> When evidence exists, I can understand what I actually bought — not just where I paid.

## Product scope

- Receipt ingestion.
- Invoice ingestion.
- PDF/image/email evidence.
- Raw document storage.
- Product extraction.
- Brand extraction.
- Variant/SKU normalization when available.
- Quantity and unit extraction.
- Price/discount/tax extraction.
- Line-item allocation.
- Evidence review.

## Example

```text
Blinkit ₹2,846

Instead of only:
Groceries ₹2,846

The system may know:
- Amul Paneer — 200g × 2 — ₹180
- Milk — 1L × 4 — ₹280
- ...
```

## Architecture

```text
Bank / SMS / Email / Upload
            ↓
       Ingestion layer
            ↓
    Raw evidence storage
            ↓
   Extraction pipeline
    ├── parser
    ├── OCR/vision model
    ├── schema validation
    └── normalization
            ↓
     Candidate facts
            ↓
      Evidence review
            ↓
       Domain facts
            ↓
   Product / Merchant modules
```

## AI role

Heavy multimodal use.

Use a model gateway so document extraction is provider-independent.

Preferred output style:

```yaml
field: product.name
value: Paneer
confidence: 0.98
source: blinkit-invoice-page-1
status: extracted
```

## Important principle

Never throw away the original evidence after extraction. Store the source so future models or improved parsers can reprocess it.

## New questions enabled

- What products do I buy most?
- How much paneer did I buy?
- Which brands do I buy?
- What is my unit cost?
- How much of my grocery spend is household vs personal?

## Quality bar

- Extraction errors are detectable and correctable.
- Source evidence is traceable.
- Unknown/unavailable product information remains unknown rather than fabricated.
- Product normalization does not merge clearly distinct products incorrectly.

## Exit criteria

A useful percentage of supported document types can be turned into trustworthy line-item data, with uncertainty clearly surfaced.

## Explicitly not building

- Perfect product master data for every merchant.
- Fully automated consumer attribution.
- Long-term consumption forecasting.

---

# Version 4 — Consumption Intelligence

## Goal

Separate **purchase** from **consumption/use**.

## User promise

> The system can help me understand not only what I bought, but what I actually use and consume.

## Product scope

- Consumption records/allocations.
- Personal vs household consumption.
- Purchase-to-consumption relationships.
- Inventory approximation where useful.
- Product usage history.
- Unit-normalized quantities.
- Price-per-unit trends.
- Consumption trends over time.

## Important distinction

```text
Purchase amount ≠ personal consumption

₹5,000 groceries
→ ₹2,000 personal
→ ₹2,000 household
→ ₹1,000 still unconsumed / inventory
```

The model should permit this without forcing precision the user does not have.

## Architecture

```text
Product events
     ↓
Consumption / Inventory layer
     ↓
Temporal relationship model
     ↓
Analytics / derived views
```

Do not try to build a perfect inventory-management system unless the product genuinely needs it. Approximation should be a valid state.

## AI role

- infer likely consumer from history;
- estimate likely consumption patterns;
- identify unusual purchase frequency;
- normalize units and equivalent products;
- generate candidate consumption observations.

AI estimates must never be presented as exact measurements unless evidence supports exactness.

## New questions enabled

- How much paneer do I consume per month?
- Which brand do I usually consume?
- Is my price per unit increasing?
- Am I buying more food than I consume?
- What does my personal vs household consumption look like?

## Exit criteria

The system supports useful consumption analysis without requiring users to manually log every consumption event.

## Explicitly not building

- Perfect physical inventory tracking for every category.
- Health/medical claims based solely on spending data.
- Automatic attribution of every item to a specific person.

---

# Version 5 — Financial Intelligence / Analyst

## Goal

Turn the accumulated financial graph into an intelligent analytical system.

## User promise

> I can ask complex questions about my financial life in natural language and get answers grounded in my actual data.

## Product scope

- Natural-language queries.
- Financial graph querying.
- Cross-module analytics.
- Trend explanation.
- Anomaly detection.
- Personal pattern detection.
- Contextual summaries.
- Evidence-linked explanations.

## Architecture

The AI should not query raw database tables arbitrarily.

```text
User question
      ↓
Intent / query planner
      ↓
Typed financial tools
      ├── transactions
      ├── allocations
      ├── products
      ├── people
      ├── obligations
      ├── consumption
      ├── contexts
      └── analytics
      ↓
Deterministic calculations
      ↓
Structured result + evidence
      ↓
Reasoning model
      ↓
Human-readable answer
```

## AI role

Strong reasoning model for difficult questions; cheaper model for routine interpretation.

Example:

> "Why did food spending rise this month?"

AI should orchestrate analysis, but numeric totals must come from deterministic tools.

## New questions enabled

- Why did my spending increase?
- What changed compared with my normal behavior?
- Which products drive grocery growth?
- How much of my apparent spending is actually for others?
- What recurring commitments are coming?
- Which spending patterns look unusual?

## Trust model

Every answer should be traceable to underlying facts or clearly labeled estimates.

For important answers, preserve enough metadata to answer:

> "Why did the system say this?"

## Exit criteria

The assistant can answer a meaningful set of real questions with low hallucination and strong traceability.

## Explicitly not building

- autonomous transfers;
- autonomous investing;
- irreversible financial actions without explicit authorization;
- a chatbot that can bypass the domain/query layer.

---

# Version 6 — Predictive and Proactive Financial System

## Goal

Move from explaining the past to helping users understand and prepare for the future.

## User promise

> The system notices important changes and helps me reason about what may happen next.

## Product scope candidates

- cash-flow forecasting;
- recurring commitment forecasting;
- goal trajectory prediction;
- consumption forecasting;
- scenario simulation;
- what-if analysis;
- proactive anomaly alerts;
- adaptive financial summaries;
- richer assets/liabilities;
- tax-aware and planning-oriented modules where legally and technically appropriate.

## Architecture

```text
Historical financial graph
          ↓
Feature / aggregation layer
          ↓
Deterministic forecasting where appropriate
          +
Statistical / ML models
          +
LLM reasoning layer
          ↓
Scenarios / forecasts
          ↓
User-facing explanation
```

Forecasting models should be separate from LLMs. LLMs explain and orchestrate; statistical/ML methods or deterministic rules produce forecast values.

## AI role

- select relevant signals;
- explain forecast drivers;
- construct what-if scenarios;
- detect meaningful changes;
- personalize how findings are communicated.

## Exit criteria

The system can produce useful, calibrated forward-looking insights with transparent uncertainty and without presenting forecasts as guarantees.

## Explicitly not building by default

- autonomous movement of user money;
- opaque financial advice presented as certainty;
- automatic high-stakes decisions.

---

# Cross-version architecture evolution

| Area | V0 | V1 | V2 | V3 | V4 | V5 | V6 |
|---|---|---|---|---|---|---|---|
| Database | PostgreSQL | PostgreSQL | PostgreSQL | PostgreSQL + object store | PostgreSQL + derived views | PostgreSQL + query layer | Add analytics/ML infrastructure only if justified |
| Backend | Modular prototype | Modular monolith | Modular monolith | Modular monolith + jobs | Modular monolith + jobs | Modular monolith + typed query tools | Modular monolith + forecasting workers |
| AI | Experiments | Light classification | Inference/defaults | Multimodal extraction | Consumption inference | Reasoning/orchestration | Prediction + proactive reasoning |
| Search | DB search | DB search | DB search | DB + evidence search | DB + product search | Query/tool layer | Add specialized search only if needed |
| Evidence | Basic metadata | Import metadata | User assertions | Raw docs + provenance | Consumption evidence | Answer traceability | Forecast provenance |
| UX complexity | Internal | Very low | Optional enrichment | Review-driven | Insight-driven | Query-driven | Proactive but controllable |

# Version gating rules

A version is ready to ship only when all of the following are true:

1. **User value exists independently.** The version should not be justified solely because it enables a future version.
2. **Earlier simplicity is preserved.** Basic users should not be forced into new concepts.
3. **The domain remains coherent.** New concepts should extend the model rather than create contradictory parallel concepts.
4. **Unknown is supported.** Missing data must remain a valid state.
5. **AI is bounded.** AI confidence/provenance exists for inferred or extracted data.
6. **Deterministic truth remains deterministic.** Arithmetic, balances, settlement, and authoritative state transitions are performed by code.
7. **Scenarios pass.** Newly relevant real-world scenarios are represented and tested.
8. **Operational cost is justified.** New infrastructure must have a measurable reason to exist.

# What can be built in parallel

The versions are a product sequence, not a strict engineering waterfall.

Parallel work can include:

- V3 document-ingestion experiments during V2.
- AI model benchmarking before V3 becomes production scope.
- Scenario expansion continuously.
- Product taxonomy experiments without making the taxonomy mandatory.
- Query/analytics prototypes before V5.

However, production dependencies should follow the architectural ordering above.

# Recommended first implementation sequence

For a solo/home project:

```text
1. V0 domain + scenarios
2. V1 usable core
3. V2 people/defaults
4. V3 one excellent document source (not every source)
5. V4 one high-value consumption category
6. V5 natural-language analyst
7. V6 only after real usage proves the need
```

The strongest practical strategy is to **go deep on one end-to-end path** before broadening:

> Bank transaction → Blinkit order → invoice → products → person allocation → personal consumption → question/insight

If that path works elegantly, the architecture is probably growing in the right direction.
