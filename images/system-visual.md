# The Agentic AI Product Framework — Visual

This visualises the **reviewed, agentic** version of the framework end to end. It
combines four bodies of work, all of which are *yours* (the SINGULARITY / Kirk
material was reference only and is excluded):

1. **The framework** — the original two loops, shared hub, and satellites
   (README / `ai-pf-01..12`).
2. **The harness** — eight agents that *staff* the framework
   ([[implementation-proposal]], [[agents]]).
3. **The closure** — the feedback loops the review found missing
   ([[analysis]]).
4. **The engineering lens** — the four disciplines every agent blends, and where
   the system will fail ([[fourcriticalgaps]], [[transferrable_rules]]).

> **Colour key:** green = knowledge loop · blue = decision loop · black = shared
> hub · grey = infrastructure (Conductor) · orange = human (PM) · purple =
> spans both loops (Challenger).
>
> **Naming note:** the delivery agent is **Courier** across all diagrams and the
> RACI (alias "Distributor" appears in the prose proposal — same agent).

---

## 1. The whole system in one diagram

The framework says *what* happens (the steps). The harness says *who does it*
(the agents). The PM owns every gate. The Conductor routes everything and
produces no content.

```mermaid
flowchart TB
    SRC["Signal in — interviews · meetings · metrics<br/>tickets · sales notes · exec voice notes"]:::green

    %% ---------- KNOWLEDGE LOOP ----------
    subgraph KL["KNOWLEDGE LOOP  (continuous)"]
        direction TB
        CAP["Capture<br/><b>Scribe</b>"]:::green
        AUTH["Author<br/><b>Drafter</b>"]:::green
        DIST["Distribute<br/><b>Courier</b>"]:::green
    end

    %% ---------- SHARED HUB ----------
    subgraph HUB["SHARED HUB"]
        direction TB
        STAGE["Staging / Factory<br/>drafts land here first"]:::infra
        STORE[("Store — single source of truth<br/><b>Custodian</b> maintains")]:::hub
        REVIEW{{"Review — human gate<br/><b>Challenger</b> pre-screens · <b>PM</b> decides"}}:::hub
        ASK["Ask Store<br/>plain-language pull"]:::blue
    end

    %% ---------- DECISION LOOP ----------
    subgraph DL["DECISION LOOP  (event-driven)"]
        direction TB
        STRAT["Strategy<br/><b>Strategist</b>"]:::blue
        DECIDE["Decide<br/><b>Strategist</b> frames · <b>PM</b> calls"]:::blue
        VALID["Validate<br/><b>PM</b> runs · <b>Analyst</b> supports"]:::blue
        MEAS["Measure<br/><b>Analyst</b>"]:::blue
        LEARN["Learn<br/><b>Analyst</b>"]:::blue
    end

    %% ---------- SATELLITES ----------
    MEMO["Decision Memo<br/><b>Drafter</b> writes · <b>Challenger</b> red-teams"]:::blue
    RISK["Risk & Trade-off<br/><b>Strategist</b> owns · <b>Challenger</b> tests"]:::blue

    PM(["PM — reviews · decides · owns"]):::human
    COND{{"Conductor — routes work, holds session state,<br/>fires triggers, enforces gates"}}:::infra

    %% Knowledge flow (draft -> staging -> gate -> store)
    SRC --> CAP --> STORE
    STORE --> AUTH --> STAGE --> REVIEW
    REVIEW -->|approved| STORE
    STORE --> DIST -->|team-shaped outputs| TEAMS["Teams<br/>eng · exec · sales · support"]:::green

    %% Decision flow
    STORE --> STRAT --> DECIDE --> VALID --> MEAS --> LEARN
    LEARN -->|lesson| STRAT
    DECIDE -. reads/writes .-> STORE

    %% Satellites
    DECIDE --> RISK --> REVIEW
    VALID --> MEMO --> SMALL["Small circle<br/>eng lead · commercial · PM peer"]:::blue

    %% Pull routes
    STORE --- ASK -.->|sourced answer or 'I don't know'| PM
    REVIEW --- PM

    %% Conductor orchestrates (dashed = control, not content)
    COND -.->|routes ×N| CAP
    COND -.->|routes ×N| AUTH
    COND -.->|routes ×N| DIST
    COND -.->|routes| STRAT
    COND -.->|routes| MEMO

    classDef green fill:#84c65a,stroke:#4f7d33,color:#0b2200;
    classDef blue fill:#0d76eb,stroke:#0a4f9c,color:#ffffff;
    classDef hub fill:#111111,stroke:#000000,color:#ffffff;
    classDef infra fill:#9aa0a6,stroke:#5f6368,color:#111111;
    classDef human fill:#f5a623,stroke:#b9781a,color:#241300;
```

---

## 2. The four execution loops (cadence · trigger · gate)

The system runs **four** loops, not two. The original two are joined by the two
the review formalised. Each has a different cadence and a different trigger, but
every one passes a human gate.

```mermaid
flowchart LR
    subgraph K["KNOWLEDGE  · continuous"]
        direction LR
        k1["new signal"]:::green --> k2["Capture -> Store -> Author"]:::green --> k3{{"Review: PM approves"}}:::hub --> k4["Distribute"]:::green
    end
    subgraph D["DECISION  · event-driven"]
        direction LR
        d1["strategy change /<br/>opportunity / Learn output"]:::blue --> d2["Strategy -> Decide -> Validate -> Measure -> Learn"]:::blue --> d3{{"Review: PM decides"}}:::hub
    end
    subgraph R["RISK VALIDATION  · event-triggered"]
        direction LR
        r1["Revisit Trigger fires<br/>(date or condition)"]:::blue --> r2["did the risk materialise?<br/>did mitigation hold?"]:::blue --> r3{{"PM re-evaluates<br/>accepted risk"}}:::hub
    end
    subgraph S["SYSTEM HEALTH  · scheduled (weekly/monthly)"]
        direction LR
        s1["Custodian cadence"]:::infra --> s2["audit Store: stale ·<br/>duplicate · orphaned · schema"]:::infra --> s3{{"Custodian acts,<br/>PM informed"}}:::hub
    end

    K --> HUB[("Store")]:::hub
    D --> HUB
    R --> HUB
    S --> HUB

    classDef green fill:#84c65a,stroke:#4f7d33,color:#0b2200;
    classDef blue fill:#0d76eb,stroke:#0a4f9c,color:#ffffff;
    classDef hub fill:#111111,stroke:#000000,color:#ffffff;
    classDef infra fill:#9aa0a6,stroke:#5f6368,color:#111111;
```

**The gaps this closed** (from [[analysis]]): the original framework *described*
Risk→Learn, System Health, and Distribute→Capture but never drew them as closed
loops — so revisit triggers rot, Store decays, and team feedback depends on
someone choosing to speak up. Loops 3 and 4 above formalise the first two; the
Distribute→Capture pull ("did this land?") and the Memo→Decide gate ("do these
comments block or merely inform?") are shown next.

---

## 3. The pull loops the review added (degradation signals)

Generation was strong (AI drafts → human reviews). What was missing were the
signals that tell you something has gone **stale, unread, or contested**.

```mermaid
flowchart LR
    subgraph P1["Distribute -> Capture  (pull)"]
        direction LR
        a1["Distribute"]:::green --> a2["team acts / reacts"]:::green --> a3["'did this land?' poll"]:::green --> a4["Capture (<b>Scribe</b>)"]:::green --> a5[("Store")]:::hub
    end
    subgraph P2["Memo -> Decide  (gate)"]
        direction LR
        b1["Decision Memo out"]:::blue --> b2["small-circle feedback<br/>via Capture"]:::blue --> b3["<b>Strategist</b> synthesises<br/>into resolution options"]:::blue --> b4{"blocker raised?"}:::blue
        b4 -->|yes| b5["Decide re-evaluates<br/>before Validate continues"]:::blue
        b4 -->|no| b6["informs, does not block"]:::blue
    end
    classDef green fill:#84c65a,stroke:#4f7d33,color:#0b2200;
    classDef blue fill:#0d76eb,stroke:#0a4f9c,color:#ffffff;
    classDef hub fill:#111111,stroke:#000000,color:#ffffff;
```

---

## 4. Orchestration & parallel spawning

The Conductor is infrastructure — it decides *when* and *where*, never *what*.
Much of the work fans out into parallel stateless subagents (×N).

```mermaid
flowchart TB
    COND{{"CONDUCTOR<br/>cadence · routing · gate enforcement · escalation"}}:::infra

    COND -.->|new signal / poll| SC1["<b>Scribe</b> ×N<br/>parallel capture:<br/>API · transcripts · tickets"]:::green
    COND -.->|post-Review, Distribute| DR1["<b>Drafter</b> ×N<br/>parallel author:<br/>roadmap · team update · summary"]:::green
    COND -.->|Distribute event| CO1["<b>Courier</b> ×N<br/>parallel deliver:<br/>WhatsApp · Jira · email · Slack"]:::green

    ST["<b>Strategist</b>"]:::blue -.->|Decide record created| CH["<b>Challenger</b><br/>red-team before Memo"]:::span
    AN["<b>Analyst</b>"]:::blue -.->|Measure cadence fires| SC2["<b>Scribe</b><br/>pull fresh metrics -> Store"]:::green
    CU["<b>Custodian</b>"]:::green -.->|System Health cadence| AN2["<b>Analyst</b><br/>Store-quality audit"]:::blue

    CH --> PM(["PM gate"]):::human
    DR1 --> PM
    ST --> PM
    AN --> PM
    PM -->|approve / reject / decide| COND

    classDef green fill:#84c65a,stroke:#4f7d33,color:#0b2200;
    classDef blue fill:#0d76eb,stroke:#0a4f9c,color:#ffffff;
    classDef span fill:#7b4fe0,stroke:#4b2c94,color:#ffffff;
    classDef infra fill:#9aa0a6,stroke:#5f6368,color:#111111;
    classDef human fill:#f5a623,stroke:#b9781a,color:#241300;
```

---

## 5. The four engineering disciplines (where the real work — and the bugs — live)

Cassandra's review reframed the harness: every agent is a **blend** of four
engineering disciplines, and *the boundaries between agents are where production
bugs live first*. This is the "Revised Layer Map (post-Cassandra)".

- **Context Engineering** — what information gets surfaced (retrieval, chunking, freshness).
- **Prompt Engineering** — how the agent behaves (tone, adversarial framing, output shape).
- **Harness Engineering** — how the system moves (routing, gates, timers, error handling).
- **Boundary Engineering** — what crosses between agents / from humans (interface design). *Added after the review — the four critical gaps all live here.*

```mermaid
/stop/
```

### The four boundary failures to engineer against

```mermaid
flowchart LR
    g1["<b>1 · Misroute cascade</b><br/>Conductor routing needs a<br/>classification layer, not keyword match"]:::bnd
    g2["<b>2 · Toothless Challenger</b><br/>needs retrieval of contradictory<br/>evidence, not just adversarial prompts"]:::bnd
    g3["<b>3 · Unowned human input</b><br/>'ship it, but watch latency' =<br/>approval + condition + risk flag; parse it"]:::bnd
    g4["<b>4 · Lossy handoffs</b><br/>Strategist -> Drafter: what crosses?<br/>full memo drowns, summary loses 'why'"]:::bnd
    classDef bnd fill:#b23c17,stroke:#7a2810,color:#ffffff;
```

---

## 6. The enforcement spine (governance that keeps it from degrading)

The review's core verdict: *the framework's design is sound; what it lacks is
**enforcement architecture** — the mechanisms that stop it degrading when agents
or humans take shortcuts.* Ten patterns (from [[transferrable_rules]]) run
across every layer as deterministic code, not prompts.

```mermaid
flowchart TB
    subgraph SPINE["Cross-cutting enforcement (deterministic-first)"]
        direction LR
        e1["Golden Source<br/>write-boundary hooks"]:::gov
        e2["Factory Staging<br/>draft -> stage -> gate -> Store"]:::gov
        e3["Write Boundaries<br/>agents can't write outside scope"]:::gov
        e4["Context Tiering<br/>always / role / conditional / on-demand"]:::gov
        e5["Provenance<br/>every output links its evidence"]:::gov
        e6["REFLEXION<br/>Learn -> tagged lesson -> recalled next time"]:::gov
        e7["Post-Mortem Audit<br/>independent check of agent output"]:::gov
        e8["Session Handoff<br/>PM re-engages without context loss"]:::gov
    end
    SPINE --> ALL[("applies to every agent<br/>and every loop")]:::hub
    classDef gov fill:#4a148c,stroke:#2e0e57,color:#ffffff;
    classDef hub fill:#111111,stroke:#000000,color:#ffffff;
```

Priority (effort ÷ impact): **Deterministic-First** and **Factory Staging** are
low-effort/high-impact — do them first. Write Boundaries, Context Tiering, and
REFLEXION are the high-impact structural changes after that.

---

## 7. A typical week (the system running)

Where the PM actually touches the work — everything else is agent-driven.

```mermaid
sequenceDiagram
    autonumber
    participant PM as PM (human)
    participant C as Conductor
    participant Sc as Scribe
    participant Cu as Custodian
    participant St as Store
    participant Dr as Drafter
    participant Ch as Challenger
    participant Str as Strategist
    participant An as Analyst
    participant Co as Courier

    Note over Sc,St: Mon — Capture & triage
    Sc->>St: ingest weekend signal (auto, ×N)
    Cu->>St: dedup, flag stale, schema check

    Note over C,Ch: Tue — Author & challenge
    C->>Dr: pending Decision Memo detected
    Dr->>Ch: pre-test memo (draft in staging)
    Ch->>PM: "blast radius understated" (surfaced, not fixed)
    PM->>Co: edit + approve -> circulate to small circle

    Note over Sc,Str: Wed — Feedback & synthesis
    Sc->>St: capture small-circle replies
    Str->>PM: 3 resolution options (no anchoring)
    PM->>St: choose option 3 + record rationale

    Note over PM,An: Thu — Validate & measure
    PM->>An: run test on narrowed cohort
    An->>St: metric designed up front, measurement live
    C-->>C: session handoff written (context limit)

    Note over An,Co: Fri — Learn & distribute
    An->>St: learn-forward tag (7-day window)
    Co->>PM: team-shaped outputs per channel (×N)
    PM->>St: Ask Store answers "what did we decide?"
```

---

## 8. Old framework → agentic system (the map)

| Framework step / property | Agent(s) | R owner | PM role |
|---|---|---|---|
| Capture | **Scribe** | Scribe | Informed |
| Store + System Ownership | **Custodian** | Custodian | Informed |
| Author + Decision Memo | **Drafter** | Drafter | Accountable |
| Review (adversarial) | **Challenger** | Challenger | Accountable |
| Review (editorial) | — | **PM** | Responsible |
| Strategy + Decide (framing) | **Strategist** | Strategist | Accountable |
| Decide (final call) | — | **PM** | Responsible |
| Validate | **Analyst** (support) | **PM** | Responsible |
| Measure + Learn | **Analyst** | Analyst | Accountable |
| Risk & Trade-off | **Strategist** (Challenger tests) | Strategist | Accountable |
| Distribute + Channel Routing | **Courier** | Courier | Accountable |
| Routing / cadence / gates | **Conductor** | — (infra) | — |
| Ask Store (pull) | Custodian indexes | — | Informed |

**The through-line is unchanged:** AI does the heavy lifting, a person owns every
decision, everything traces back to one Store. The review's contribution is
turning *described intentions* into *mechanisms* — named owners (§1, §8), closed
loops (§2–§3), an orchestration model (§4), an engineering lens that predicts
where it breaks (§5), and an enforcement spine that stops it decaying (§6).
