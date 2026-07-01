---
pii: none
type: framework-extension
---
# Transferable rules: the enforcement architecture

The framework's design is sound. What it lacks is enforcement — the mechanisms that stop the system degrading when agents or humans take shortcuts. These ten rules are that layer. They are drawn from operational experience running an agentic system in production, and each is expressed here in this framework's own terms.

They pair with the [design pattern library](../design-patterns/design-patterns.md) and the [agentic systems](../agentic-systems.md) doc: the patterns name what is possible, these rules say what to enforce and in what order.

## The disciplines blend at the boundaries

The clean split between context, prompt, and harness engineering is a useful mental model, but every real agent is a blend. The ratio differs:

- **Scribe and Custodian** are context-engineering-heavy — getting the right information in and out of Store
- **Drafter, Challenger, and Courier** are prompt-engineering-heavy — controlling output quality and shape
- **Conductor** is harness-engineering-heavy, but needs context engineering to route well
- **the boundaries between agents** are where all three meet, and where most production bugs live

The verdict that governs everything below: **the system fails silently at boundaries before it fails loudly anywhere else.** Budget engineering effort accordingly — the interfaces between agents matter more than the agents themselves.

## The ten rules

### 1. Deterministic-first

Prefer code over LLM reasoning for any repeatable task. A repeatable task solved by a prompt is a recurring token cost; solved by a script, it is a one-time cost. Most of the harness layer should be deterministic.

Where it applies:

- Capture routing (signal → correct bucket) is a rule on source type and metadata tags, not semantic classification
- gate enforcement at Review is a boolean, "approved yes or no", not a judgment call
- Distribute channel routing is a recipient → format lookup table, not a generation task
- revisit-trigger firing is a date comparison or event match, not "does the AI think it's time?"

### 2. Golden source

Each data type has exactly one authoritative location; everything else is generated from it. This is the framework's core idea, but it needs enforcement:

- decision records live in Store, not in a Slack thread *and* Store
- the Decision Memo is generated from Store, never authored independently
- Ask Store reads from one index, not several caches

The moment someone edits a Distribute output instead of editing Store and regenerating, the system breaks. Enforce it with write-boundary hooks — code that blocks writes to downstream locations (see rule 8).

### 3. Context tiering

Information reaches an agent at exactly one tier. Higher tiers are cheaper and always present; lower tiers are richer and require a lookup. Never duplicate across tiers.

| Tier | Contents | Loaded |
|------|----------|--------|
| 1 — Always | strategy objectives, team channel preferences, access-control rules | every agent, every call |
| 2 — Role | agent-specific instructions (the Drafter's tone rules, the Challenger's framing) | per agent |
| 3 — Conditional | triggered context (a "roadmap update" task injects the last few decision records) | on task match |
| 4 — On-demand | full evidence lookup from Store | only when needed |

Without tiering, every call loads the whole of Store — token waste and retrieval noise. This is the first practical problem you hit at scale.

### 4. Staging before Store

AI output goes to a staging area first. A person reviews and accepts before it moves to Store. Staging is stateless — no memory between runs.

This maps directly to the Author → Review → Store flow:

- the Drafter writes to staging, never straight to Store
- Review is a gate (accept, reject, edit), not a suggestion
- rejected drafts do not pollute Store history

Without staging, one bad Author output contaminates Store, and every downstream consumer — Ask Store, the next Drafter call, Distribute — inherits the error. Staging makes "guardrails keep the record honest" structural rather than vague.

### 5. REFLEXION — active recall

When something fails, extract the lesson, tag it, and make sure it surfaces at the next relevant decision. Lessons are recalled at session start and attached to related work.

This extends the Learn step. Today it says "compare prediction to outcome, the gap is the lesson" — but the lesson needs somewhere to go:

- store it as a typed record, not loose narrative
- tag it to the strategy objectives it relates to
- surface it automatically the next time the Strategist frames options in that area
- track whether lessons are actually applied — recurrence means the lesson was not learned

Without active recall, Learn produces documents nobody reads. The [[sos]] protocol forces recall before work begins: the Strategist must see relevant lessons before framing new options.

### 6. Post-mortem audit

An independent function audits other agents' outputs against expectations. It reports findings without operational authority and cannot be pressured by the agent it audits.

The framework has no quality-assurance function today. Who checks whether:

- the Drafter's outputs actually match Store evidence (hallucination)?
- the Challenger's challenges are evidence-backed rather than generic?
- a Distribute output actually landed and was read?
- Learn's conclusions are sound rather than confirmation bias?

Add an audit function — it need not be a separate agent; a periodic harness job works — that samples outputs and verifies citations trace to real Store records, Decision Memos hold the riskiest assumption rather than a safe one, and Learn conclusions are supported by the data.

### 7. Decision logging with rationale

Every decision records what was decided, why, what was rejected, and when to revisit. The Decide step already records "why the other options were deprioritised" — extend it to a consistent shape:

| Field | Purpose |
|-------|---------|
| Decision | what was chosen |
| Rationale | why this over the alternatives |
| Rejected options | what was not chosen, and why |
| Assumptions | what must stay true for this to be correct |
| Revisit trigger | when to re-evaluate |
| Confidence | high, medium, or low |
| Evidence quality | evidence-rich or a gut call |

A decision log like this prevents re-litigation of settled calls: "we already decided this, here is why, and here is what would need to change to reopen it."

### 8. Write-boundary enforcement

Agents cannot write outside their allowed paths, enforced by deterministic hooks rather than by trusting the prompt:

- Scribe writes to Store; cannot write Distribute outputs
- Drafter writes to staging; cannot write directly to Store
- Courier writes to delivery channels; cannot modify Store
- Challenger writes challenge records; cannot modify the decision it is challenging

Without write boundaries, an agent error cascades. A Drafter that writes straight to Store bypasses Review; a Courier that "adapts for channel" by changing content introduces drift from source. Enforce with code, not trust.

### 9. Provenance tracking

Every output traces back to its origin — which evidence, which decision, which agent produced it. Ask Store's three rules (only from Store, cite the source, admit gaps) are provenance at the query layer. Extend it to all outputs:

- every Distribute document links back to the Store records it was generated from
- every Decision Memo traces to its Decide record and Validate evidence
- every Learn conclusion links to the Measure data behind it

When someone asks "why does the roadmap say X?", the answer should be one hop, not an archaeology project.

### 10. Session handoff

At session end, generate a handoff capturing what was done, what is pending, and what state things are in. The next session starts by reading it.

The PM does not work continuously. When they return, they need to know what signals arrived, what drafts wait for Review, what Decision Memos got responses, and what revisit triggers fired. A structured handoff — not a notification flood — surfaces pending actions, new signals, and state changes since last interaction. This is the write half of the [[eos]] / [[sos]] pair.

## Priority: what to build first

Effort against impact. Do the low-effort, high-impact rules first.

| # | Rule | Effort | Impact | Why now |
|---|------|--------|--------|---------|
| 1 | Deterministic-first | Low | High | Do first — halves token cost |
| 4 | Staging before Store | Low | High | Structural — prevents Store contamination |
| 8 | Write boundaries | Medium | High | Prevents cascade failures |
| 3 | Context tiering | Medium | High | Solves "what does the agent see?" at scale |
| 5 | REFLEXION / active recall | Medium | High | Makes Learn compound |
| 7 | Decision logging | Low | Medium | Prevents re-litigation |
| 2 | Golden source | Low | Medium | Framework states it; enforcement is the gap |
| 6 | Post-mortem audit | Medium | Medium | Quality assurance without slowing flow |
| 9 | Provenance | Medium | Medium | Enables debugging and trust |
| 10 | Session handoff | Low | Medium | PM re-engagement without context loss |

The framework's intellectual design is sound. What it lacks is this enforcement architecture — the mechanisms that keep the system from degrading when agents or humans take shortcuts.
