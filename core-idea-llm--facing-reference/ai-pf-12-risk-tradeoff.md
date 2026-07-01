---
pii: none
source: https://github.com/jesscancode/ai-product-framework
ingested: 2026-07-01
type: external-framework
parent: "[[index]]"
section_id: "12"
section_title: "Next Steps: A Risk and Trade-off Layer"
---
# Next Steps: A Risk and Trade-off Layer

Everything above answers *what did we decide and why*. It doesn't yet answer *what did we knowingly accept to get there*.

Right now **Decide** sets the direction and **Validate** tests it, but the cost of a choice rarely gets written down in the same disciplined way. It lives in the PM's head — the same problem the whole framework exists to remove elsewhere.

## Position in the Framework

**Risk & Trade-off** sits alongside **Decide**, the way **Decision Memo** sits alongside **Validate**. **Author** drafts it the moment a **Decide** record is created, pulling from **Store** — known dependencies, how similar past decisions played out, how thin or solid the evidence behind this one actually is. The PM reviews it the same way they review everything else: reads, edits, decides, owns it.

[Diagram: Risk & Trade-off layer — showing its position alongside Decide in the decision loop]

## Record Shape

Each record holds a small, consistent shape:

| Field | Description |
|-------|-------------|
| **The risk** | Named plainly |
| **Reversibility** | A one-way door or a two-way door — this alone should set how much scrutiny it gets |
| **Blast radius** | Who or what breaks if this is wrong |
| **The call** | Accept, mitigate, avoid, or transfer — stated outright |
| **The reasoning** | In the PM's own words, so it's a decision and not just a checkbox |
| **An owner** | Who is responsible |
| **Revisit Trigger** | A date or condition that forces a second look, so the record doesn't just sit there |

## Circulation

For anything irreversible or wide-blast-radius, it can circulate like **Decision Memo** does — out to the small circle before the trade-off is locked in, so someone else gets a chance to catch it while it's still cheap to change.

## Revisit Trigger and Learning

**Revisit Trigger** is what keeps this clear over time. It feeds **Learn** exactly the way **Measure** does: did the risk we accepted actually happen, did the mitigation hold, etc. That answer goes back into **Store**, so the next **Risk & Trade-off** record **Author** drafts is a little sharper than the last one — creating a growing memory of what this team's trade-offs actually cost.

## Integration with Ask Store

It plugs into **Ask Store** for free. *"What did we accept when we shipped X"* or *"what were we assuming about Y"* becomes answerable the same way *"what did we decide about X"* already is — sourced, or it says it doesn't know.

## The Fourth Debt

Documentation debt is decisions going stale. Strategic debt is decisions being re-argued. Technical debt is teams acting on mismatched information.

This is **risk debt** — the cost of a choice arriving later as a surprise, to someone who never got to weigh in on whether it was worth taking.

See also: [[ai-pf-06-decision-loop]], [[ai-pf-07-decision-memo]], [[ai-pf-11-content-design]]
