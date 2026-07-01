---
name: feos
description: Fast End of Session. Minimal wrap-up for a micro-session (under 30 minutes, under 3 items, nothing committed). Run when EOS classifies the session as Micro.
---
# Fast end of session (FEOS)

Minimal wrap-up for a micro-session. No audit, no formal session note, no commit — because a micro-session produces nothing that needs them. Run this when [[eos]] classifies the session as Micro.

## Procedure

**1. Note**
- 1.1 Append a one-line micro-session entry to the previous session note: `Micro-session <time>: <summary>`.

**2. Handoff**
- 2.1 Update the Conductor's handoff with a brief summary, so the next session still starts from current state.

**3. Loose ends**
- 3.1 Confirm nothing was left mid-gate: no draft stranded in staging, no half-captured signal outside Store.

If the session produced a lesson, a decision outcome, or a committed record, it was not a micro-session — stop and run [[eos]] instead.
