---
id: data-truth-model
name: Data Truth, Provenance and Uncertainty
status: active
audience: [human, agent]
authority: stable
last_updated: 2026-08-30
---

# Data Truth, Provenance and Uncertainty

## Purpose

Financial software becomes dangerous when it cannot distinguish what actually happened from what the system merely assumes.

This document defines the conceptual trust model.

## Knowledge states

### Fact

Authoritative or explicitly established information.

Examples:

- bank feed says ₹2,846,
- user explicitly says brother was beneficiary,
- receipt explicitly lists Amul paneer 400g.

### Extracted fact

A fact obtained by parsing a source where the extraction itself is sufficiently reliable and can be traced back to evidence.

### User assertion

Information directly stated or corrected by the user.

User assertions generally outrank inference.

### Default

A reusable assumption that is applied in the absence of a more specific fact.

Example:

> Paneer purchases are usually for me.

### Inference

A hypothesis derived from available evidence or historical patterns.

Example:

> This protein powder is probably for brother.

### Estimate

A deliberately approximate value.

### Unknown

The system does not know enough to make a reliable statement.

## Provenance

Every important non-trivial value should be able to answer:

> Where did this come from?

Useful provenance fields conceptually include:

```yaml
source: bank-feed | sms | email | invoice | image | user | rule | model | history
source_reference: optional identifier
method: explicit | extracted | normalized | inferred | defaulted | estimated
created_at: timestamp
model: optional model identifier
confidence: optional numeric or categorical value
based_on: optional references to supporting facts
```

Exact storage can differ.

## Authority order

A practical conceptual order is:

1. Explicit user correction/assertion.
2. Direct authoritative source evidence.
3. High-quality extracted evidence from trusted documents.
4. Deterministic rules with known provenance.
5. Personalized defaults.
6. AI inference.
7. Estimate.
8. Unknown.

This is not a universal numerical ranking; conflicting evidence should be resolved using event/source semantics rather than a blind hierarchy.

## Conflicting evidence

If sources conflict, preserve the conflict.

Example:

```text
Bank source: ₹2,846
Invoice: ₹2,796
```

The system should not silently overwrite one value merely to make the record tidy.

It should be possible to represent:

- both sources,
- the discrepancy,
- resolution status,
- and any final derived value.

## User corrections are knowledge

When a user changes:

> consumer = household

to:

> consumer = me

that correction can be stored as an explicit fact for the current event and as a candidate learning signal for future similar events.

Do not retroactively rewrite every historical event unless an explicit bulk correction is intended.

## Defaults should not masquerade as facts

The UI may make defaults feel seamless, but the internal model should remain able to distinguish:

```text
Explicit: user said “for brother”
Default: historically, this product is usually for brother
Inference: this particular purchase appears to follow that pattern
```

## Confidence is not truth

A confidence score expresses model certainty under a particular method.

It does not mean:

> 0.95 = 95% guaranteed correct.

Use confidence as a decision-support signal, not as a substitute for provenance.

## Unknown is valuable

An honest unknown can later become known when new evidence appears.

A fabricated value can contaminate analytics and training forever.

## Derived values

Calculations such as:

- net expense,
- current receivable,
- monthly consumption,
- savings rate,
- category totals,
- trend percentages

should be derived deterministically from underlying facts/relationships whenever practical.

Derived results should be traceable back to inputs.

## Immutable history principle

Historical events should generally be append-only or mutation-controlled.

Returns, reversals, refunds, corrections, and later payments should be represented through explicit relationships where possible.

## Trust rule

> **When the system is uncertain, show or preserve the uncertainty rather than manufacturing precision.**
