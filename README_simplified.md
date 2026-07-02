# Agentic product management, in plain terms

This is a plain-language companion to the [full README](./README.md). It explains the same system in the order that is easiest to read the first time. For the complete set of diagrams, agent roles, and build detail, read the [full README](./README.md).

**What this is.** A way of running product management work so that AI does the heavy lifting and a person still owns every decision.

**Who it is for.** A product manager — the person who decides what a product team builds next — and the teams who depend on those decisions. Product manager is shortened to PM from here on.

**What you get from reading on.** The core idea, the problem it removes, and how the whole thing fits together. If you only want the concept, read the next three sections and stop.

## Start here: the idea at a glance

![Linear illustration of the workflow: eleven steps running top to bottom — Strategy, Capture, Store, Decide, Validate, Author, Review, Distribute, Teams, Measure, Learn — closing on an Outcome, with three sidecars beside their steps and an enforcement band running underneath.](./images/linear_example.svg)

> **For illustration only. The real system is not a straight line.** The picture above straightens the workflow into a single top-to-bottom pass, so you can learn the steps and the words in order. The real system loops back on itself. Team feedback flows back to the start. Each lesson learned reshapes the next decision. Treat this as a primer, not the finished design. The looped version is in the [full README](./README.md).

## The idea in one line

Make each decision once, write it down in one place, and generate everything else — every document, every update — from that one record.

Hold one record that everyone trusts. Produce all the rest from it. That single move is what the whole system is built around.

## The problem it removes

Every product team leaks in the same four ways:

- decisions live in one person's head, so they get re-argued or quietly reversed
- documents drift out of date, so people act on stale information
- sales promises one thing while the roadmap says another, and engineering absorbs the gap
- insight arrives in a quarterly rush instead of arriving as it happens

This is a missing system, not a people problem. Faster documents do not help if the team is acting on the wrong decision. The fix is a loop: make a better call, record it once, deliver it to each team in a form they will use, measure what happens, and feed that back in.

## A worked example

An exec gets a short roadmap update on WhatsApp. They reply with a four-minute voice note. That reply is transcribed automatically, flows into the system, and lands in the shared record as evidence. It then reshapes the next decision.

Ask that same person to log into a tool and fill in a form, and you would have got silence. The channel is what makes the feedback happen at all. So the system treats each person's channel as part of the design, not an afterthought.

## How the system works

Three things carry the whole system: one shared record, two loops, and one human gate.

### One shared record

At the centre sits **Store** — the single source of truth. It is the one place the real evidence and reasoning live. Every document is generated from it, so no two teams ever work from conflicting copies. You can also ask Store a plain-language question at any time and get an answer drawn from the real documents.

### Two loops

Two cycles run through Store. Each takes signal in and puts a result back:

- the **knowledge loop** turns raw signal into documents and sends them to the teams, and it runs continuously
- the **decision loop** sets a goal, makes the call, tests it, measures the result, and learns from it, and it runs when something prompts it

The knowledge loop moves through Capture, Store, Author, Review, and Distribute. The decision loop moves through Strategy, Decide, Validate, Measure, and Learn. In every diagram, green marks the knowledge loop and blue marks the decision loop.

### One human gate

Both loops pass through the same checkpoint: **Review**. A person reads, edits, decides, and approves. Nothing leaves without human judgment behind it. This is the core promise — AI drafts, a person owns the decision.

## Four loops keep it from going stale

The two loops above are joined by two more that keep the system honest over time. All four write back to the shared record.

| Loop | What it does | When it runs |
|------|--------------|--------------|
| Knowledge | turns signal into documents and sends them out | continuously |
| Decision | sets a goal, makes the call, tests it, and learns | when a goal changes or a chance appears |
| Risk check | re-checks a risk you accepted earlier | on a set date or condition |
| Housekeeping | cleans the shared record | on a schedule, weekly or monthly |

Because each one is a closed loop, nothing quietly rots. Reminders fire on time, the record stays clean, and team feedback comes back as signal rather than depending on someone choosing to speak up.

## Three extras around the record

Three smaller pieces sit next to Store, each covering a need the two main loops do not.

**Decision memo.** Before a decision is settled, the PM shares a dated snapshot of the current thinking with a small circle — an engineering lead, a commercial peer, another PM. Their job is to pressure-test it, not to sign it off. It goes out once before the test and once after. Comments come back in as ordinary signal.

**Ask the record a question.** Anyone can ask Store a plain-language question and get a sourced answer. Three rules keep it honest: it answers only from documents actually in Store, it names and links the document it used, and it says so plainly when nothing in Store covers the question.

**Risk and trade-off.** Deciding sets the direction, but the *cost* of a choice rarely gets written down with the same care. This record fixes that. It names the risk, says whether the choice is reversible, states who breaks if it is wrong, records the call and the reasoning, and sets a date to look again. It exists so the cost of a decision never arrives later as a surprise.

## The teams feed the system, both ways

The teams are not the end of the line. The same people who receive documents are the freshest source of new signal, so the arrow runs both ways. The system sends to each person in their preferred format, and it takes feedback back in that same format. Meeting people where they already are is what makes the feedback happen.

## Who does the work: eight AI agents

An **agent** is a small AI worker with one clear job. Eight of them staff the system — enough to give every step a clear owner, without piling unrelated jobs onto one overloaded worker.

| Agent | What it does |
|-------|--------------|
| Scribe | captures incoming signal and transcribes it |
| Custodian | keeps the shared record clean and organised |
| Drafter | writes documents from the shared record |
| Courier | delivers each document in the right channel |
| Strategist | frames the options and objectives for a decision |
| Analyst | designs the metric, measures it, and draws out the lesson |
| Challenger | argues against the plan to find the holes before the PM sees it |
| Conductor | routes work to the others and writes nothing itself |

Two rules hold across all of them:

- the PM owns every decision, because agents draft, challenge, and organise, but a person reviews and approves
- the shared record is the single source of truth, so no agent keeps a private version that contradicts it

A solo PM at low volume does not need all eight on day one. The smallest working setup is Store, Scribe, and Drafter, with the PM playing the other roles. Add each agent when its absence starts costing the PM real attention.

## What keeps it honest

A sound design still decays when people or agents take shortcuts, unless something holds it in place. A set of fixed rules runs underneath every step to prevent that:

- staging: drafts wait in a holding area until a person approves them
- write boundaries: each agent can only change its own area
- provenance: every output links back to the evidence behind it
- active recall: each lesson learned is tagged and brought back next time
- independent audit: a separate check on quality, rather than self-marking
- clean handoff: the PM can step away and return without losing the thread

These are rules the system enforces, not habits it hopes people keep.

## Where to start building

You do not build everything at once. This order follows what depends on what, and each phase produces something usable on its own.

| Phase | Build | You get |
|-------|-------|---------|
| 1 | Store, Scribe, Custodian | a working knowledge base that takes in and maintains signal |
| 2 | Drafter, Conductor | documents generated from the record, with basic routing |
| 3 | Challenger | a hard review before the PM sees anything |
| 4 | Strategist, Analyst | a working decision loop with measurement and learning |
| 5 | Courier | delivery in each team's own channel |
| 6 | Ask Store | plain-language questions answered from the record |

## Why this is content design, not admin

It is easy to mistake this for tidy paperwork. It is not. The discipline behind it is [content design](https://contentdesign.london/blog/why-content-design-exists): decide what the reader actually needs, and the best form to meet it, before writing a word. Most teams point that at customers. This system points it inward, at the PM's own process.

One shared record with generated outputs removes four kinds of hidden cost at once:

- documentation debt: documents go stale and start to contradict each other
- strategic debt: decisions get re-argued, or built over
- technical debt: teams act on mismatched information, and someone patches the gap under deadline
- risk debt: the cost of a choice arrives later as a surprise, to someone who never got to weigh in

Removing all four is what lets a team move fast without the mess piling up. This is a product built for the people who build products.

## Where to go next

- the [full README](./README.md) for every diagram, the agent responsibilities in detail, and the enforcement patterns
- the [engineering companion](./ENGINEERING-COMPANION.md) for how the agents are staffed, governed, and safely extended
- the [content design principles](./content_design_principals/universal-content-design-rules.md) this system is built on
