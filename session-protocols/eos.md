---
name: eos
description: End of Session. Run when the PM ends a working session, so lessons, decisions, and state are written back to Store before they are lost.
---
# End of session (EOS)

This is the write half of the framework's session-handoff pattern; [[sos]] is the read half. EOS captures what the session produced — lessons, decision outcomes, a handoff — so the next session starts from a clean state and nothing lives only in the PM's head.

## Session classification

Classify first, then route.

| Condition | Tier |
|-----------|------|
| Under 30 minutes, under 3 items touched, no records committed | **Micro** → run [[feos]] instead |
| PM says "eos — no audit" | **Standard** (override) |
| PM says "eos — audit" | **Full** |
| Session over 2 hours | **Full** |
| A major finding flagged during the session | **Full** |
| Everything else | **Standard** |

Priority: an explicit PM override beats the duration and findings triggers, which beat the default.

## Standard EOS

**0. Classification gate**
- 0.1 Classify the session. If Micro, run [[feos]]. If Full, use the Full EOS procedure below.

**1. REFLEXION**
- 1.1 Review the session: decisions made, items worked, patterns observed.
- 1.2 Identify lessons, mistakes, and reusable patterns.
- 1.3 Write each lesson as a typed record in Store, tagged to the strategy objective it relates to, so it surfaces the next time that objective is in play.

**2. Update decision records**
- 2.1 For any decision whose outcome is now observable, update its record: what happened against what was predicted, and whether an accepted risk materialised.

**3. Session note**
- 3.1 Write a short session note to Store: what was done, what was decided, what is pending.
- 3.2 Reference any lessons or decision updates made above.

**4. Cross-domain transfer**
- 4.1 Note any pattern from this session that applies to another product area or team, so it is not relearned there from scratch.

**5. Handoff**
- 5.1 Write the Conductor's handoff: what was done, what is pending, and what context the next session needs to pick up cleanly.

**6. Commit to Store**
- 6.1 Confirm every draft approved this session has passed the Review gate and been written to Store. Nothing stays in staging.
- 6.2 Confirm provenance links are intact on new records.

## Full EOS

Identical to Standard EOS, except step 1 is replaced with an independent audit.

**1. Post-mortem audit**
- 1.1 Write a short summary of the session's actions: decisions made, items worked, key changes.
- 1.2 Hand the summary to the audit function (an independent pass, not the agent that produced the work).
- 1.3 The audit checks: do citations trace to real Store records, do Decision Memos hold the riskiest assumption rather than a safe one, are Learn conclusions supported by the data.
- 1.4 The audit writes its findings to Store and returns a findings count and severity.
- 1.5 On failure or timeout, note "audit skipped — reason" in the session note and continue.

Then, in step 3 (Session note), also record the audit's findings count and link, confirm any REFLEXION lessons the audit proposed, and record any decision outcomes it surfaced.
