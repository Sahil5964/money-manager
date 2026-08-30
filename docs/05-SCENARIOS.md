---
id: real-world-scenarios
name: Real-World Scenarios
status: active
audience: [human, agent]
authority: evolving
last_updated: 2026-08-30
---

# Real-World Scenarios

These scenarios are domain tests. They are deliberately messy. New architecture should be checked against them before adding special-case behavior.

## S01 — Minimal purchase

**Situation:** User wants only basic tracking.

> ₹500 at a known merchant.

**Known:** amount, date, merchant.

**Unknown:** category, people, products, context.

**Expected:** valid record; no required extra questions.

**Questions enabled:** basic cash-flow/spending questions.

## S02 — Family grocery purchase

**Situation:** User pays ₹3,000 for groceries for the household.

Some items are personally consumed, some are household, and one item is for a sibling.

**Important distinction:** payer = user; beneficiaries/consumers are mixed; exact ownership may not matter.

**Expected:** support household/shared allocation without forcing artificial percentages.

## S03 — Brother's item in mixed order

**Situation:** Blinkit order contains a ₹900 protein powder that is normally bought for brother.

**System behavior:** infer brother allocation based on learned history; preserve as inference/default, not fact, until confirmed when appropriate.

## S04 — Reimbursement expected

**Situation:** User pays ₹2,000 for a friend and expects ₹2,000 back.

**Expected:** cash outflow + receivable relationship.

The ₹2,000 should not necessarily count as final personal spending.

## S05 — Family support / no reimbursement

**Situation:** User pays ₹2,000 for mother and does not expect money back.

**Expected:** cash outflow + beneficiary relationship + gift/support semantics if the user chooses to track them.

## S06 — Maybe reimbursed

**Situation:** User pays ₹1,500 for a sibling and may or may not ask for reimbursement later.

**Expected:** model uncertainty around the obligation rather than forcing loan vs gift immediately.

## S07 — Gift

**Situation:** User buys an ₹8,000 phone for father.

**Expected:** payer = user; beneficiary = father; no receivable; asset/product may exist; intent = gift if known.

## S08 — Friend pays first

**Situation:** Friend pays ₹5,000 restaurant bill. User's share is ₹1,500.

**Expected:** payable relationship to friend; no false cash outflow until user actually pays.

## S09 — Credit card purchase + later settlement

**Situation:** User makes purchase on August 2 and pays the credit card on August 20.

**Expected:** purchase and card-payment events are related; second event is not another expense.

## S10 — Transfer between own accounts

**Situation:** ₹50,000 moved from HDFC to ICICI.

**Expected:** transfer; no expense and no income.

## S11 — Refund

**Situation:** ₹4,000 purchase, ₹1,500 item returned three days later, ₹1,500 refunded.

**Expected:** preserve original purchase and related return/refund events; net cost can be derived.

## S12 — Partial refund

**Situation:** ₹10,000 order receives ₹1,000 partial refund.

**Expected:** related refund; original order remains historically accurate.

## S13 — Failed / reversed payment

**Situation:** payment initially appears completed but is reversed.

**Expected:** lifecycle state and related reversal; final economic outcome should not double-count.

## S14 — Annual subscription

**Situation:** ₹12,000 paid for one year.

**Expected:** cash flow today; optional consumption allocation over 12 months; recurring commitment relationship.

## S15 — Advance payment

**Situation:** ₹20,000 paid in advance for a service. Final bill later is ₹17,000 and ₹3,000 is refunded.

**Expected:** advance/prepayment relationship, final expense, refund.

## S16 — Discount + cashback

**Situation:** Product list price ₹1,200, coupon reduces price by ₹200, cashback of ₹50 arrives later.

**Expected:** preserve components; allow views for paid cost and net economic cost.

## S17 — Product-level grocery analysis

**Situation:** Blinkit invoice identifies:

- Amul paneer 200g × 2
- milk 1L × 2
- vegetables
- cleaning supplies

**Expected:** item-level facts can coexist with grocery transaction.

**Questions enabled:** product quantity, brand, price, category, consumption.

## S18 — Consumption differs from purchase

**Situation:** User buys 2kg paneer for household over multiple days.

**Expected:** purchased quantity and consumed quantity are separate concepts.

## S19 — Asset purchase

**Situation:** User buys an ₹80,000 laptop.

**Expected:** cash outflow + asset acquisition; optional later depreciation/consumption concept.

Do not force it to behave exactly like a disposable grocery expense.

## S20 — Shared internet bill

**Situation:** ₹1,200 internet bill benefits four people.

**Expected:** shared household benefit without requiring 25% ownership semantics.

## S21 — Trip context

**Situation:** User spends ₹80,000 during a Europe trip across flights, hotel, food, transport, and shopping.

**Expected:** normal categories remain intact while events can additionally belong to a trip context.

## S22 — Business expense

**Situation:** User pays ₹4,000 for a work expense expected to be reimbursed by employer.

**Expected:** business/work context + receivable relationship; personal spending reports can optionally exclude it.

## S23 — Unknown merchant

**Situation:** Bank description is `AMZN Mktp IN* 3499`.

**Expected:** merchant normalization may infer Amazon; product identity may remain unknown until evidence appears.

No fabricated product.

## S24 — Conflicting evidence

**Situation:** Bank transaction says ₹2,846. Invoice says ₹2,796 due to a separate payment adjustment.

**Expected:** preserve sources and flag discrepancy rather than overwriting one with the other silently.

## S25 — User correction

**Situation:** AI infers paneer was for user; user says it was for parents.

**Expected:** transaction-level truth changes according to user assertion; the correction can become a learning signal for future inference.

## S26 — Unknown consumer

**Situation:** Household buys a snack with no useful information about who consumes it.

**Expected:** consumer can remain unknown.

## S27 — Cash spending with weak evidence

**Situation:** User withdraws ₹5,000 cash and later remembers only that ₹700 was spent on lunch.

**Expected:** cash withdrawal is not automatically treated as spending; later cash expense can be recorded separately.

## S28 — Currency conversion

**Situation:** €100 purchase settles to ₹9,200 plus ₹150 FX fee.

**Expected:** preserve original currency, settlement amount, and fee separately enough to support analysis.

## S29 — Tax / fee components

**Situation:** Restaurant bill has food ₹2,000, taxes ₹360, service charge ₹100, tip ₹200.

**Expected:** optional detailed components without requiring them for basic expense tracking.

## S30 — Subscription price change

**Situation:** Service was ₹699/month and becomes ₹799/month.

**Expected:** recurring commitment history can expose change over time.

## Acceptance principle for scenarios

A scenario passes conceptually when the product can represent it without:

- requiring irrelevant fields,
- destroying historical truth,
- collapsing distinct roles into one person field,
- treating every allocation as a percentage,
- or inventing information that is not known.
