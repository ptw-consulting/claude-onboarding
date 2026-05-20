---
name: Always update session log
description: At the end of every Claude session, append a dated summary to project_session_log.md
type: feedback
---

At the end of every conversation, append a new dated entry to `project_session_log.md` summarizing what was accomplished.

**Why:** The user wants continuity across sessions — future Claude instances need to know what was done and where things stand without re-asking.

**How to apply:** When the conversation reaches a natural stopping point (the user says "thanks," "great," "we're done," or signals end-of-session), append a `## YYYY-MM-DD — short title` block at the top of `project_session_log.md` with 2-5 bullets covering: what was done, decisions made, anything left open.
