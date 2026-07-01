---
pii: none
source: https://github.com/jesscancode/ai-product-framework
ingested: 2026-07-01
type: external-framework
parent: "[[index]]"
section_id: "04"
section_title: "The Whole Workflow, in One Diagram"
---
# The Whole Workflow, in One Diagram

[Diagram: The master view — full workflow running top to bottom, showing both loops and their shared hub, updated to include the decision memo loop and the Store query node]

The workflow runs top to bottom. Two loops move through it, and they share one hub.

**The knowledge loop** moves raw signal into a shared record, turns it into documents, and sends those out to the teams.

**The decision loop** sets the goal, makes the call, tests it, measures the result, and learns from it.

Both loops read from and write to the same hub, called **Store**. Both pass through one human checkpoint, called **Review**. Colour tells the two apart in every diagram: green for the knowledge loop, blue for the decision loop.

Two more things sit close to **Store**, each with its own section:

- A **Decision Memo** that circulates early, while a call is still being tested (see [[ai-pf-07-decision-memo]])
- A way to query **Store** directly, in plain language, at any time (see [[ai-pf-08-ask-store]])

See also: [[ai-pf-05-knowledge-loop]], [[ai-pf-06-decision-loop]]
