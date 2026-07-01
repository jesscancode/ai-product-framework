---
pii: none
source: https://github.com/jesscancode/ai-product-framework
ingested: 2026-07-01
type: external-framework
parent: "[[index]]"
section_id: "07"
section_title: "The Decision Memo"
---
# The Decision Memo: A Provisional Distribution, While the Call Is Still in Flight

[Diagram: The decision memo loop — Validate branching right to the memo, out to a small circle, back through Capture]

**Distribute** is not the only point where a document leaves the system. At **Validate**, before a decision is settled, a **Decision Memo** is circulated: a timestamped snapshot of the thinking so far. The riskiest assumption, how it is being tested, what is assumed, what is expected to happen.

## Audience

This goes to a small circle, not the whole company: engineering, product peers, and the management layer above. Their job is to pressure-test the thinking, not to sign off on a finished decision.

## Two Versions

It runs in two versions, not one:

1. **Before the test** — with the assumptions and the test plan, so the circle can catch a bad test before it costs any effort.
2. **After the test** — with what was found and what is now thought, for a final check.

Both versions use **Author**'s existing guardrails: dated, and hard to quietly rewrite after the fact.

## Feedback Loop

Whatever comments come back are signal, so they come in the same way as everything else: through **Capture**, then into **Store**, sharpening the decision before it is finalised.

## Decision Memo vs. Distribute

The difference between **Decision Memo** and **Distribute** is temperature, not mechanism:

- **Distribute** is wide and settled: it tells a large audience a decision is made.
- **Decision Memo** is narrow and provisional: it asks a small audience to find the holes before the decision is made.

See also: [[ai-pf-06-decision-loop]], [[ai-pf-08-ask-store]], [[ai-pf-12-risk-tradeoff]]
