---
pii: none
source: https://github.com/jesscancode/ai-product-framework
ingested: 2026-07-01
type: external-framework
parent: "[[index]]"
section_id: "08"
section_title: "Ask Store Directly"
---
# Ask Store Directly

[Diagram: A small sidecar node beside Store, labelled "ask a question," with its three rules shown as a short legend]

**Distribute** is one route out of **Store**: push, on a schedule, shaped for each team. There is a second route: pull, on demand, in plain language. Anyone can **Ask Store** a direct question — "what did we decide about X", "what's the status of Y" — and get an answer sourced from the real documents.

## Three Rules

Three rules keep this honest:

1. It answers only from documents that are actually in **Store**, never from general knowledge.
2. Every answer names and links to the document it came from.
3. If nothing in **Store** covers the question, it says so, rather than guessing.

## Scope Boundary

This only reads from the approved layer of **Store**, after **Review**, never from drafts still in **Author**. A half-finished draft is not yet true, and this must never present it as if it were.

## Compound Benefit

The benefit compounds. Small, in-the-moment questions no longer wait for a scheduled **Distribute** or a message to the product manager. The source of truth becomes something a person can interrogate directly, not something only one person can navigate.

See also: [[ai-pf-05-knowledge-loop]], [[ai-pf-10-properties]]
