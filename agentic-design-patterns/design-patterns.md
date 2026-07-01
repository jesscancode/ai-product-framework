# Design patterns for AI systems

A working library of patterns for building agentic systems. Some are wired into this framework already (the [README](../README.md) and [agentic-systems](../agentic-systems.md) show where). Others are kept here because they are worth reaching for when the system grows, even if nothing uses them yet. Each entry names the pattern, describes it in general terms, and — where one exists — points to how this framework applies it.

**How to read the status tags:**

- **[active]** — used in this framework today
- **[available]** — a sound pattern, not yet wired in; kept for when it earns its place
- **[rejected]** — evaluated and deliberately not used, recorded so it is not re-litigated

## Structural

- **Golden source** *[active]* — one authoritative location per data type; everything else is generated from it. In this framework, Store is the golden source, and downstream outputs are regenerated rather than edited in place.
- **Single entry point** *[available]* — all control flows through one route rather than many side doors. The Conductor is the analogue: work enters and routes through one orchestrator, not agent-to-agent back channels.
- **Least privilege** *[active]* — each agent gets only the tools, context, and write paths its mandate needs, never a blanket grant.
- **Framework–data separation** *[active]* — the shareable framework (agents, prompts, routing) is kept separate from private user data (the evidence in Store). A classification marker on each record enforces the boundary at the file level, so sensitive material never leaks into shareable config.
- **Spec-driven development** *[available]* — requirements → design → tasks, settled before implementation begins. Maps to the decision loop's discipline: Strategy frames, Decide commits, and only then does Validate build.
- **Structural supremacy** *[active]* — fix the structure, not the behaviour. Constrain the system's shape with deterministic rules so correct behaviour follows, rather than correcting outputs one at a time. This is the enforcement spine's whole premise.
- **Source hierarchy** *[available]* — a tiered document precedence (authority > governance > reference > procedure > analysis > evidence > ephemeral); when sources conflict, the higher tier governs. A record's type marks its tier. Useful once Store holds enough conflicting material to need adjudication.
- **Hybrid domain integration** *[available]* — a sub-domain runs as a nested system with its own Store section, tasks, and owner, but shares infrastructure and stateless workers with the parent. Neither owner crosses the boundary. The path to multi-PM or multi-product-area scaling.
- **Split-document ingestion** *[available]* — large external documents are split into a meta file (provenance plus a linked table of contents) and individual section files, so agents load only the relevant section rather than the whole monolith.
- **Domain bootstrap** *[available]* — an atomic vertical slice that creates an agent's config, its Store section, its skills, its RACI row, and its index entries in a single commit — so there is never an intermediate state where components reference peers that do not yet exist. See the agent creation checklist in [agentic-systems](../agentic-systems.md).
- **Deterministic document extraction** *[available]* — external documents (PDF, DOCX) enter Store through code, not agent discretion. Three stages: **extract** (a format-specific script converts source to raw sections using deterministic rules), **stage** (output lands in staging), **ingest** (reviewed content moves to Store with metadata and links added). Extraction is repeatable; agents only get involved after staging. Split-document ingestion defines the output shape; this defines the mechanism that produces it.

## Behavioural

- **RACI matrix** *[active]* — clear ownership for every workflow: one Responsible, the PM Accountable for decisions, others Consulted or Informed.
- **Ritual bookends** *[active]* — session protocols ([[sos]] / [[eos]]) enforce discipline at the start and end of a working session; each session compounds on the last through writes to Store and the handoff.
- **Handoff pattern** *[active]* — session context persists in the Conductor's handoff: what was done, what is pending, what state things are in. The next session reads it before proceeding.
- **Conviction disclosure** *[available]* — every routing or decision call carries an explicit confidence and decisiveness score, so a low-conviction call is visible as such rather than passing silently.
- **Expected-value annotation** *[available]* — a lightweight expected-value decomposition attached to a decision, making the implicit cost-benefit explicit and reviewable.
- **Iterative siege** *[active]* — if an approach fails twice, stop patching and try a fundamentally different one. Mirrors the framework's failure handling: two rejections of the same step means fix the agent, not re-run it.
- **Semantic compaction** *[active]* — completed work is dead context. The handoff compresses finished work to preserve budget for live work.
- **Pointer versus payload** *[available]* — when a system injects context automatically, it should inject pointers (a path plus why it is relevant, ~10–20 tokens) not full payloads, and let the agent pull the content on demand. Every injected byte is unremovable context; payloads waste it when the agent does not need them. Rule of thumb: if the automatic output is over 50 tokens, it is probably a payload — convert it to a pointer stating what exists and why, not what it contains.
- **Memory architecture** *[available]* — distinct memory types (procedural, knowledge, reflexive, episodic, working) with tiered retrieval; the source hierarchy governs precedence when retrieved results conflict. The framework's REFLEXION and context-tiering patterns are two slices of this.
- **Diagnostic check** *[active]* — a three-question premise audit before routing a complex task: is this the actual problem or a symptom, is it pitched at the right level, does a baseline already exist?
- **Session-busy flag** *[available]* — when one session starts bulk changes, it sets a flag; other sessions check it before touching shared records and skip if set. Prevents one session reading Store mid-write. The flag auto-expires so a crashed session does not block forever.
- **Style library** *[active]* — calibrated voice profiles loaded as behavioural constraints on document-producing agents. Each profile is source-grounded, versioned, and prescriptive — it tells the agent how to write, not what the voice "is like". This is how the Drafter and Courier shape output per audience.
- **Delegation response contract** *[available]* — when one agent consults another, the prompt fixes the response shape: either a direct answer or a structured delegation block the caller can execute. Prevents lossy handoffs (see the boundary failures in [agentic-systems](../agentic-systems.md)).
- **Progressive file assembly** *[available]* — for work exceeding one agent's context, break it into sub-questions, one agent per question, each appending to a shared file. The file is the accumulator; no single agent holds everything at once.
- **Supersession tracking** *[available]* — a structured record of what was superseded, what replaced it, and where, logged in both the affected document and a project-level log. Makes lifecycle transitions visible and queryable — the disciplined form of the framework's fight against documentation debt.
- **Batch config normalisation** *[available]* — periodic reformatting of all agent configs to one style, plus atomic propagation of cross-cutting changes to every agent at once, so diffs show only semantic changes.

## Safety

- **Defence in depth** *[active]* — layered protection rather than one gate: secret scanning, classification markers, and pre-commit checks, so no single failure exposes sensitive material.
- **Dry-run first** *[active]* — always preview before a mutation. In the framework, drafts stage and pass the Review gate before they touch Store.
- **Multi-remote** *[available]* — push to all configured backups so work is never lost to a single point of failure.
- **Honesty protocol** *[active]* — accuracy over helpfulness: state what was checked and what could not be verified. Ask Store embodies this — it answers from Store or says it does not know.
- **Robustness bias** *[active]* — when efficiency and robustness conflict, favour robustness.
- **Stop-think-act-review checks** *[available]* — explicit checkpoints before high-risk actions. The triggers are prescriptive; the response is contextual.
- **Tool-failure circuit breaker** *[active]* — after two consecutive failures on the same call, stop retrying and diagnose the root cause. The framework's harness-failure handling is this pattern.
- **Logical fallacy checklist** *[available]* — a validation checklist for knowledge claims covering classical fallacies, LLM-specific patterns, and operational fitness; applied during research extraction and adversarial review. Ammunition for the Challenger.
- **Deterministic hook escalation** *[active]* — governance graduates from soft to hard enforcement as violations recur: prompt instruction → written rule → observability hook (logs violations) → blocking hook (prevents them). Escalate when a rule is broken more than once or the cost justifies it. Not everything needs a hook — each one fires on every call, adding cost. Anti-pattern: hookifying a rule that has never been violated.
- **Read-before-write gate** *[active]* — a hook blocks edits to a record until it has been read in the current session. Deterministic enforcement of investigate-before-modify; exempts creation, since you cannot read what does not exist yet.
- **Index coverage verification** *[available]* — a script that checks every record is linked from its parent index and reports orphans, so nothing goes missing from navigation. The deterministic backstop for the Custodian.

## Agent patterns

- **Supervisor delegation** *[active]* — the orchestrator routes work and never implements directly. The Conductor decides when and where, never what.
- **Skeptic review** *[active]* — every significant decision gets challenged by an agent with an adversarial mandate, run in true isolation so its judgment is not coloured by the work it reviews. This is the Challenger.
- **Store enrichment** *[active]* — research and findings are written to Store, so every agent benefits from what one learned.
- **Reference examples** *[available]* — raw analyses of external systems are kept as source material, separate from the promoted findings distilled from them.
- **Context engineering** *[active]* — prompting is asking; context engineering is equipping. The right information, tiered and injected, matters as much as the instruction. One of the four disciplines.
- **Deep research loop** *[available]* — a structured research pipeline: causal chaining, gap identification, steelman checks.
- **Agentic absorption** *[available]* — systematically mine external systems for transferable patterns. This document is itself a product of that pattern.
- **Agent swarm** *[active]* — parallel execution via stateless subagents. The Conductor fans work out ×N (many Scribes capturing, many Couriers delivering).
- **Gated task procedure** *[active]* — a structured change process with human gates at the explain and report points; relevant paths travel with the work so context follows the task.
- **Agent tool scoping** *[active]* — specialist agents get only the tools their role requires, never a wildcard grant. Full tool access on a specialist overflows its context: tool descriptions consume base context that cannot be compacted, triggering reload loops. Keep specialist prompts lean; only orchestrators and implementers get broad access.

## Planning patterns

Three academic planning patterns, implemented through governance rather than bespoke code, named here for cross-reference with the literature.

- **Gated delegation** *[active]* (Plan-and-Execute) — separate planning from execution with explicit phase boundaries: scope → design → build → review. Implemented through the gated task procedure and RACI delegation.
- **Reflexive learning** *[active]* (Reflexion) — verbal reinforcement learning from failure: post-mortem → tagged lesson → recall at the next relevant decision. The framework's REFLEXION pattern; the loop closes at session start ([[sos]]).
- **Adversarial consultation** *[available]* (Multi-Perspective / Debate) — consult agents with opposing mandates before committing, in true isolation so their attention does not bleed together. The Challenger is one voice of this; a fuller debate (design defended, ethics evaluated) is available when stakes justify it.

## Prompt engineering patterns

Each pattern has three modes:

- **Pattern** — a framing that steers output toward a specific profile
- **Neutral** — a deliberately unbiased framing that minimises framing effects
- **Anti-pattern** — a framing known to degrade output quality or predictability

### Presupposition loading

Embed desired behaviours as assumptions rather than instructions. Shifts output toward applied, contextualised analysis.

- **Pattern:** "Given that you've already surveyed the major frameworks, assess which patterns are most applicable."
- **Neutral:** "Survey the major frameworks. Assess which patterns are applicable."
- **Anti-pattern:** "You probably already know the frameworks, so just tell me which ones work." (Presupposes knowledge without grounding; invites confabulation.)

Trade-off: gains contextualisation, loses empirical breadth.

### Precision anchoring

Use specific, unambiguous language to constrain output format and scope. Eliminates interpretation variance.

- **Pattern:** "List exactly 5 patterns. For each: name (≤5 words), applicability score (1–10), one-sentence rationale."
- **Neutral:** "List the patterns you find relevant and explain why."
- **Anti-pattern:** "Tell me about some patterns that might be useful." (Vague scope invites unbounded output.)

Trade-off: gains structure and comparability, loses exploratory breadth.

### Pace-and-lead

Establish shared context before introducing the task. Mirrors few-shot prompting — the pacing builds a frame the model continues.

- **Pattern:** "This framework uses eight agents with RACI-governed workflows, a single Store, and a human gate on every decision. Given this architecture, identify the boundaries most likely to fail."
- **Neutral:** "Identify the failure points of a multi-agent product system."
- **Anti-pattern:** "You know how our system works. What breaks?" (Assumes shared context without establishing it; the model fills gaps with assumptions.)

Trade-off: gains relevance and specificity, costs tokens on context establishment.

### Task reframing

Shift the frame of the task to change the register of the output. The same work, described differently, produces differently shaped results.

- **Pattern:** "Audit these decision records for unstated assumptions" (frames as quality assurance — produces structured findings with severity).
- **Neutral:** "Analyse these decision records" (frames as open analysis — produces balanced assessment).
- **Anti-pattern:** "Fix these decision records" (frames as remediation before diagnosis — the model skips analysis and jumps to changes).

Trade-off: powerful steering tool, but the framing choice is itself a bias. Choose consciously.

### Double-bind choice

Offer constrained alternatives that all lead to useful output. Preserves model autonomy within bounds.

- **Pattern:** "Should we standardise team-update formats for consistency, or keep per-team formats for fit? Assess both."
- **Neutral:** "Assess the trade-offs of standardising team-update formats."
- **Anti-pattern:** "We should standardise the formats, right?" (Leading question — the model confirms rather than analyses.)

### Cognitive frame

A two-to-four-sentence section at the top of a skill file that sets the agent's mental posture before the procedure begins. Structurally a pace-and-lead applied to on-demand skill content, not always-on prompts.

- **Pattern:** "When you begin a review, you shift from passive reading to forensic analysis. You already know that surface-level correctness often masks structural problems. You read for contradiction, not confirmation."
- **Neutral:** Omit the cognitive frame entirely — proceed directly to the procedure steps.
- **Anti-pattern:** "You are the best reviewer. You always find every error. Nothing escapes you." (Identity-reinforcing superlatives; over-steering activates the model's representation of the opposite.)

Apply to analytical skills (research, review, audit, design). Skip for mechanical ones (the session protocols, routine runs). Cap presuppositions at half the procedure steps or fewer. No identity-reinforcing closers.

Trade-off: gains analytical posture, costs tokens on framing. Does not apply to agent prompts or skill descriptions.

## Pipeline composition

The orchestrator composes multi-stage pipelines by selecting from a library of templates. Each template defines its stages, agent assignments, dependency wiring, and gate criteria.

**When to use:** any task needing more than one specialist agent in sequence or parallel.

**How it works:** the orchestrator selects the template matching the task's complexity and risk, fills in task-specific parameters, and dispatches the stages. The pipeline is chosen *before* execution begins — plan the full workflow, then hand off. No mid-pipeline improvisation.

| Pipeline | Stages | Use case |
|----------|--------|----------|
| **DCBV** | Design → Challenge → Build → Verify | Full rigour: new features, agent creation, infrastructure changes |
| **Build-and-review** | Build → Review | Fast path: implementation where no spec is needed |
| **Parallel-specs** | N × Spec (parallel) | Multiple independent specs sharing no dependencies |
| **Research** | Research → Review | Knowledge tasks: pattern extraction, investigation |

**Key principle:** pipeline selection is a planning decision, not a runtime one. Gated delegation provides the theory; pipeline composition provides the concrete template library.

## DCBV pipeline (Design → Challenge → Build → Verify)

The full-rigour pipeline for high-risk or complex changes. Four stages with explicit handoffs and true isolation between them. In this framework the stages map naturally onto the agents:

1. **Design** (Strategist) — produce the spec, schema, or decision record
2. **Challenge** (Challenger) — adversarial review: find overlaps, gaps, failure modes, and request fixes. The job is to find problems, not to approve.
3. **Build** (Drafter, or the PM for a validation test) — implement the approved design with the challenge fixes applied
4. **Verify** (audit function) — check the implementation against the design constraints and return pass or fail with findings

**When to use:** new architecture, agent creation, decisions that are expensive to undo, cross-cutting changes.

**When not to use:** simple tasks, documentation, config changes, work with a clear spec already. Use build-and-review instead.

**Key principle:** the Challenge stage exists to catch design flaws *before* implementation cost is incurred. Isolation between stages prevents sunk-cost bias — the Challenger reviews a design, not a half-built system. This is the same logic as the Decision Memo's pre-test version.

## Rejected anti-patterns

Patterns evaluated and deliberately rejected, recorded so they are not re-evaluated.

- **Learning on stateless subagents** *[rejected]* — subagents are stateless, so a bad output is fixed by changing the prompt or skill, not by teaching the agent. Runtime prompt patching and per-agent memory apply only to an orchestrator with session continuity.
- **Conditional steering** *[rejected]* — loading governance rules conditionally by file match, when the rule set is small enough to load unconditionally. The conditional machinery costs more than it saves until the rule count is large.
- **Single capability manifest** *[rejected]* — one file declaring every agent's capabilities. It was not actually a single source of truth and added maintenance burden; distributed per-agent registries won.
- **Cross-harness translation** *[rejected]* — expressing every skill in multiple tool formats. Target one harness; the abstraction overhead is unjustified until a second harness is real.
- **Three-way merge for agent reviews** *[rejected]* — parallel reviewers do not need merge; they annotate append-only or in separate files. Merge is only needed if agents edit the same prose at once, which RACI prevents.
- **Runtime hook profiles** *[rejected]* — an environment switch controlling which hooks fire (minimal/standard/strict). Overhead exceeds the saving while the hook count is small; revisit past ~30 hooks.
- **Monolithic tool dispatcher** *[rejected at scale]* — one switch routing all tools works under ~15 tools but does not scale; use a declarative schema with per-domain handlers instead.
- **Stdio transport for a persistent daemon** *[rejected]* — stdio suits a client-spawned process, not an always-on service. Use HTTP/SSE for persistent daemons.
