---
name: fsos
description: Fast Start of Session. Minimal boot for a quick, scoped task. Run instead of SOS when the PM knows the one thing they came to do.
---
# Fast start of session (FSOS)

Minimal boot for a quick, scoped task. Use this when the PM already knows the one thing they came to do and a full [[sos]] would be overhead.

## Procedure

**1. Load standing context**
- 1.1 Load the always-on tier only: strategy objectives, team channel preferences, access-control rules.

**2. Read the handoff**
- 2.1 Read the Conductor's handoff for anything that blocks the task at hand. Skip the rest.

**3. Confirm and start**
- 3.1 Say: "FSOS complete. What's the task?"

Skip everything else: the Store delta, the full Review queue, new-signal triage, revisit triggers, memo feedback, and broad lesson recall. If the task turns out to be larger than scoped, stop and run [[sos]] properly.
