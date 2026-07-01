# Agentic systems: how the framework is staffed and governed

This is the engineering companion to the [README](./README.md). The README explains the framework — the two loops, the shared hub, the satellites. This document explains the harness that staffs it: the eight agents, the principles they must hold, the disciplines that make them work, the anti-patterns that break them, and the checklist for adding a new one.

Read the README first if you have not. This document assumes you know what Capture, Store, Author, Review, Distribute, Strategy, Decide, Validate, Measure, and Learn mean.

## What makes this system agentic

The framework defines *what* happens. The harness adds *who* does it, and answers four questions the framework leaves open:

- agents have defined roles, tools, and write boundaries — no agent does everything
- an orchestration layer (the Conductor) routes work, holds session state, and fires triggers, but writes no content
- one shared record (Store) is the single source of truth every agent reads from and writes to
- deterministic code, not prompts, enforces the rules that keep the system from decaying

The through-line never changes: **AI does the heavy lifting, a person owns every decision, everything traces back to one Store.**

## The eight agents

Eight agents staff the harness — the minimum that gives each framework step a clear owner without collapsing distinct capabilities into one overloaded role. Seven do the work. The eighth routes it.

| Agent | Primary capability | Steps served | Colour |
|-------|-------------------|---------------|--------|
| **Scribe** | Ingestion and transcription | Capture | Green |
| **Custodian** | Store maintenance, dedup, schema, index | Store, System Ownership | Green |
| **Drafter** | Document generation from Store | Author, Decision Memo | Green |
| **Courier** | Channel-aware delivery and format shaping | Distribute, Channel Routing | Green |
| **Strategist** | Objectives, option framing, feedback resolution | Strategy, Decide (framing) | Blue |
| **Analyst** | Metric design, measurement, learn-forward tagging | Validate, Measure, Learn | Blue |
| **Challenger** | Adversarial review and risk surfacing | Review (adversarial), Risk & Trade-off | Purple (spans both loops) |
| **Conductor** | Routing, session state, triggers, gates | Infrastructure — no framework step | Grey |

### Why eight, and not fewer

Three simpler shapes were considered and rejected:

- **three agents** (Drafter, Challenger, Router) leaves the PM doing the measurement, strategy, and maintenance the framework exists to remove
- **five agents** (add Analyst, Custodian) buries strategic synthesis inside the Drafter's prompt; strategy is not drafting
- **twelve agents** (one per step) over-decomposes tightly coupled steps like Capture/Store and Measure/Learn, adding routing overhead without capability

### Agent mandates

- **Scribe** — receives signal in any form, normalises it, extracts metadata, writes to Store. Does not interpret or summarise.
- **Custodian** — keeps Store tidy: dedup, schema, archival, valid provenance links, and the index that makes Ask Store possible. Decides what is tidy, not what is true.
- **Drafter** — generates documents from Store with guardrails: dated to real work, hard to rewrite, shaped for the audience. Writes to staging, never straight to Store.
- **Courier** — delivers each team the document it needs, in its channel and format. Adapts format, never content.
- **Strategist** — holds the yardstick, frames options against objectives, and synthesises conflicting memo feedback into resolution options. Presents options; does not decide.
- **Analyst** — designs the success metric at the moment of decision, runs measurement, and tags the prediction-versus-outcome gap as a recallable lesson.
- **Challenger** — pressure-tests drafts and risk records before the PM sees them. Surfaces holes; does not fix them.
- **Conductor** — routes work, holds session state, fires triggers, enforces gates, and writes a handoff when a session hits its context limit. Produces no content and holds no authority.

## Principles

Two tiers. **Principles** must hold in any implementation. **Patterns** are one valid approach; a team may swap them.

### Principles that must hold

1. **Single source of truth** — Store is the only authoritative record. Every output writes to Store or derives from it.
2. **Human gate on every decision** — no agent commits a decision, publishes, or distributes without PM review.
3. **Provenance on every output** — every document, answer, and recommendation traces back to the evidence that justified it.
4. **One Responsible per workflow** — every RACI row has exactly one Responsible agent. Shared responsibility is no responsibility.
5. **Sequential per decision cycle** — a single cycle (Strategy → Decide → Validate → Measure → Learn) cannot parallelise its steps; you cannot validate what you have not decided. Separate cycles may overlap.
6. **Agents are stateless** — each receives full context at invocation and retains nothing between calls. Only the Conductor holds state, and that is infrastructure, not authority.
7. **Least privilege** — an agent gets only the tools, context, and write paths its mandate needs.

### Patterns worth adopting

- **Guardrailed drafting** — the Drafter uses templates with mandatory fields: date, source, audience, status.
- **Scheduled maintenance** — the Custodian runs a periodic pass on top of event-triggered hooks.
- **Adversarial before editorial** — the Challenger runs before the PM, so the PM sees holes already surfaced.
- **Channel as design** — the Courier treats each recipient's channel as part of the content design, not a delivery afterthought.
- **Session handoff** — near a context limit, the Conductor writes what was done, what is pending, and what the next session needs, then terminates cleanly.

## The four engineering disciplines

Every agent is a blend of four disciplines. The ratio differs by agent, and the boundaries between agents are where production bugs live first.

| Discipline | Controls | Heaviest in |
|------------|----------|-------------|
| **Context engineering** | what information gets surfaced (retrieval, chunking, freshness) | Scribe, Custodian |
| **Prompt engineering** | how the agent behaves (tone, adversarial framing, output shape) | Drafter, Challenger, Courier |
| **Harness engineering** | how the system moves (routing, gates, timers, errors) | Conductor |
| **Boundary engineering** | what crosses between agents and from humans (interface design) | every handoff |

The verdict that matters: **the system fails silently at boundaries before it fails loudly anywhere else.** Budget engineering effort accordingly — the interfaces between agents matter more than the agents themselves.

### The four boundary failures to engineer against

1. **Misroute cascade** — the Conductor routes on keyword match instead of classification, so a customer escalation lands in Capture as raw signal instead of reaching the Strategist. Fix with a lightweight classification layer.
2. **Toothless Challenger** — adversarial prompts with no adversarial evidence produce generic challenges the PM dismisses. Fix with retrieval tuned for contradictory evidence and past failures.
3. **Unowned human input** — "ship it, but watch the latency" is an approval plus a condition plus a risk flag, captured as a bare "approved". Fix with an explicit human-input parsing layer.
4. **Lossy handoffs** — the Strategist's full memo drowns the Drafter, but a summary loses the "why these options". Fix by deciding explicitly what crosses each boundary.

## The enforcement spine

The framework's design is sound; what it lacks is the enforcement that stops it degrading when agents or humans take shortcuts. These run across every layer as deterministic code, not prompts.

| # | Pattern | Effort | Impact | Do it |
|---|---------|--------|--------|-------|
| 1 | **Deterministic-first** — code over prompts for repeatable tasks | Low | High | First — halves token cost |
| 2 | **Factory staging** — draft → stage → gate → Store | Low | High | First — prevents Store contamination |
| 3 | **Write boundaries** — agents can't write outside their scope | Medium | High | Prevents cascade failures |
| 4 | **Context tiering** — always / role / conditional / on-demand | Medium | High | Solves "what does the agent see?" at scale |
| 5 | **REFLEXION** — Learn → tagged lesson → recalled next time | Medium | High | Makes Learn compound |
| 6 | **Decision logging** — decision, rationale, rejected options, assumptions, revisit trigger, confidence | Low | Medium | Prevents re-litigation |
| 7 | **Golden source** — one authoritative location per data type | Low | Medium | Enforced with write-boundary hooks |
| 8 | **Post-mortem audit** — independent check of agent output | Medium | Medium | Quality assurance without slowing flow |
| 9 | **Provenance** — every output links its evidence | Medium | Medium | Enables debugging and trust |
| 10 | **Session handoff** — PM re-engages without context loss | Low | Medium | Structured handoff, not a notification flood |

## Anti-patterns to avoid

- **Agent sprawl** — do not add an agent without a clear RACI role and a distinct capability. Eight is the ceiling until volume demands more.
- **Tool overload** — least privilege, not maximum capability. An agent with every tool has no boundary to enforce.
- **Orphan knowledge** — if it is not in Store, it does not exist. No agent keeps a private record that contradicts Store.
- **Editing the output, not the source** — the moment someone edits a Distribute output instead of editing Store and regenerating, the system breaks. Write-boundary hooks block this.
- **Factory as permanent storage** — staging is for drafts awaiting the gate, never a second home for records.
- **Skipping the gate** — Review is binary (accept, reject, edit), not a suggestion. A draft that skips it contaminates every downstream consumer.
- **Dual Responsible** — two agents R on one workflow means neither owns it. Split the workflow or pick one.
- **Toothless challenge** — a Challenger with rhetoric but no evidence gets dismissed. Give it ammunition.
- **Prompt for the deterministic** — gate checks, channel routing, and trigger firing are lookups and booleans, not judgment calls. Code them.
- **Learn that nobody reads** — a lesson that is not typed, tagged, and surfaced at the next relevant decision is not learned.

## Agent creation checklist

When adding a new agent to the harness:

1. Name the distinct capability it adds. If it overlaps an existing agent's mandate, extend that agent instead.
2. Map it to one or two framework steps. If it spans more, it is probably two agents.
3. Assign R / A / C / I for every workflow it touches. Keep one Responsible per row; the PM stays Accountable for decision-bearing work.
4. Write its mandate: what it does, what it must never do, and the questions it asks.
5. Define its write boundaries — the exact paths it may write to — and enforce them with hooks, not trust.
6. Set its context tier: what is always loaded, what is role-specific, what is conditional, what is on-demand.
7. Decide what crosses each boundary into and out of it — the full record, a summary, or a structured subset.
8. Add provenance: every output it produces must link the evidence it came from.
9. Give it a place in the loops (which loop, which cadence, which trigger) and confirm the Conductor can route to it.
10. Add it to this document's agent table and the README's agent list, and add a failure mode for it.

## Failure modes

The happy path is in the README. When things go wrong:

- **Bad agent output** — the Challenger catches factual errors, the PM catches misalignment. Rejected output does not enter Store; the Conductor routes it back with the reason. Two failures on the same input is a signal to fix the agent, not to re-run it.
- **Harness failure** — the Conductor flags any workflow pending longer than its expected step duration. The PM resumes from the last completed step; no step repeats unless its output was lost.
- **Gate rejection** — after two rejections of the same step, the Conductor stops re-routing and escalates to the PM: rewrite the brief, reassign, or handle manually. Frequent rejections mean the RACI or the agent needs re-evaluation, not lower standards.

## Cold start: scaling down to a solo PM

A solo PM at low volume does not need eight agents on day one. The minimum viable harness is **Store + Scribe + Drafter** — the PM plays Challenger, Strategist, and Analyst, which is what they already do. Add each agent when its absence starts costing PM attention: the Challenger when the same errors keep slipping through, the Strategist when option synthesis eats real time. Below the full-team threshold, fewer agents with the PM filling the gaps is not a compromise — it is the right configuration.
