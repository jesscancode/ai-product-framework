Where Subagents Make Sense

  Subagents (stateless, spawned for a task, results returned) fit where:

  - Work is parallelisable and independent
  - The task is well-scoped with clear input/output
  - No persistent state is needed between runs
  Key Design Decisions

  1. PM is R on every Review gate — the framework's core contract. AI drafts, human owns.

  2. Strategist and PM share R on Strategy/Decide — Strategy is a joint act. The agent frames options and surfaces evidence; the PM makes the call. Dual-R here is intentional — neither can produce the output
  alone.

  3. Challenger is R on Decision Memo and Risk — this is the adversarial function. It exists to find holes, not to build. Giving it R forces the system to actually generate challenge rather than making it
  optional.

  4. Conductor is A on almost everything — it's the orchestrator, not the executor. It owns cadence, routing, and gate enforcement. It never produces content.

  5. Teams are C (passive consulted)* — they generate signal by existing and reacting, not by being formally asked. The pull mechanism ("Distribute Landed?") is what converts them from passive to active
  sources.

  What This Looks Like Running

  A typical week:

  1. Scribe captures 40 signals (automated) → Store grows
  2. Conductor detects pending Decision → spawns Strategist to frame options
  3. Strategist produces options memo → PM reviews, decides
  4. Conductor spawns Challenger to red-team the decision
  5. Drafter produces Decision Memo → Courier sends to Small Circle
  6. Small Circle feedback flows through Scribe back into Store
  7. PM reviews feedback, finalises decision
  8. Drafter produces team-specific outputs → Courier delivers (parallel subagents)
  9. Analyst sets up measurement, watches metrics
  10. Custodian runs weekly Store audit, archives 3 stale items

  The PM touches steps 3, 5 (review), 6 (read), 7 (decide). Everything else is automated or agent-driven. That's the framework's promise: heavy lifting done, decisions still human.