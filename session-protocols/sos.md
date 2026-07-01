---
name: sos
description: Start of Session. Run when the PM starts a working session, so they re-engage the framework without losing context.
---
# Start of session (SOS)

This is the PM's re-engagement ritual. The PM does not work continuously — they leave and come back. SOS surfaces what changed while they were away, so no signal, decision, or revisit trigger is missed. It is the read half of the framework's session-handoff pattern; [[eos]] is the write half.

For a quick, scoped task where a full boot is overkill, run [[fsos]] instead.

## Procedure

**1. Read the handoff**
- 1.1 Read the Conductor's handoff from the last session: what was done, what is pending, what state things are in.

**2. Refresh from Store**
- 2.1 Read what changed in Store since the last session — new and modified records only, not the whole record.
- 2.2 Present a one-line summary: "Store: N added, M modified, D archived since last session."

**3. Review queue**
- 3.1 Surface drafts and decisions awaiting the PM's Review: source workflow, producing agent, and any Challenger notes already attached.
- 3.2 Present as the day's gate work: what needs approve, reject, or edit.

**4. New signals**
- 4.1 Surface signal captured since the last session: new tickets, transcripts, metrics, and exec replies now in Store.
- 4.2 Flag any that belong to an active decision cycle.

**5. Revisit triggers**
- 5.1 Check every open Risk & Trade-off record for a revisit trigger whose date or condition has fired.
- 5.2 Surface fired triggers as questions: "did this risk materialise, did the mitigation hold?" Do not auto-resolve — inform only.

**6. Memo feedback**
- 6.1 Check for small-circle responses to any Decision Memo in flight.
- 6.2 If the Strategist has synthesised them into resolution options, surface those. Flag any blocker that gates Decide.

**7. Load standing context**
- 7.1 Load the always-on tier: strategy objectives, team channel preferences, access-control rules. These are the constraints that apply to every decision this session.

**8. Recall lessons**
- 8.1 For each active decision cycle, query Store for REFLEXION lessons tagged to its objective. Surface relevant prior lessons before any new options are framed — a lesson unread at the point of decision is not learned.

**9. Present the plan**
- 9.1 Present overdue items first, then today's, with columns: item, due, owner, PM action required.
- 9.2 Do not load future-scheduled work unless the PM asks.
- 9.3 Say: "SOS complete. Where do you want to start?"
