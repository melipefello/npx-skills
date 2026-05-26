---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up. Use when wrapping up work, switching context, handing off to a fresh session, or the user says "handoff".
argument-hint: "What will the next session be used for?"
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Save it to a path produced by `mktemp -t handoff-XXXXXX.md` (read the file before you write to it).

Suggest the skills to be used, if any, by the next session.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, commits, diffs). Reference them by path instead.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.
