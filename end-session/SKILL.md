---
name: end-session
description: End-of-session documentation pass — captures decisions in memory, reviews existing memories for staleness, flags debt and follow-ups, then produces a handoff. Use when wrapping up a session, ending work, saying goodbye, or before the user leaves.
---

<what-to-do>

Perform a thorough end-of-session documentation pass. Work through each phase below, then produce a handoff for the next session.

Do not ask the user to walk you through what happened — you were in the conversation. Reconstruct decisions yourself.

</what-to-do>

<supporting-info>

## Phase 1 — Inventory decisions

Scan the full conversation for every decision, rule, preference, or correction. Categorize each into one of the four memory types:

| Signal in conversation | Memory type | Example file |
|---|---|---|
| User corrected your approach, confirmed a non-obvious choice, or stated a preference for how to work | `feedback` | `feedback_*.md` |
| You learned something about the user's role, knowledge, or personal context | `user` | `user_*.md` |
| Project status changed, a deadline was set, work was completed or blocked, a decision was made about ongoing work | `project` | `project_*.md` |
| A pointer to an external system, URL, dashboard, or tool was shared | `reference` | `reference_*.md` |

Skip anything already in memory, derivable from code/git, or only useful within this session.

## Phase 2 — Write and update memories

For each item from Phase 1:

1. **Read MEMORY.md** to check if an existing memory covers it — update rather than duplicate.
2. **Write new memory files** with proper frontmatter (`name`, `description`, `type`) and structured body. For `feedback` and `project` types, include `**Why:**` and `**How to apply:**` lines.
3. **Update MEMORY.md index** — one line per entry, under 150 chars.
4. **Convert relative dates** to absolute (e.g., "next Thursday" to the actual date).

## Phase 3 — Staleness review

Read every file referenced in MEMORY.md and verify:

- Frontmatter `description` still matches content
- No memory contradicts what happened this session
- Project memories with dates haven't expired or been superseded
- Completed items are marked done or removed

Also scan for project-level documentation that may exist in the working directory:

```
Glob pattern: **/{CONTEXT,CHANGELOG,TODO,SPEC-*,PLAN-*,ADR-*}.md
```

If any are found, flag (but don't update) sections that conflict with this session's decisions — the handoff captures these as next-session work.

## Phase 4 — Surface follow-ups

List any open threads that weren't resolved this session:

- Incomplete work or next steps the user mentioned
- Debt flagged during the session
- Questions that were deferred

If agents or skills are available (check CLAUDE.md), map follow-ups to specific agent/skill triggers so the next session can act immediately. Example: _"Say 'triage the inbox' to process the 3 notes we created."_

## Phase 5 — Ask clarifying questions (if needed)

If any decision is ambiguous or you're unsure how to document it, ask the user NOW — before writing. Keep questions minimal and concrete. If nothing is ambiguous, skip this step entirely.

## Phase 6 — Produce handoff

Once all documentation is saved, invoke `/handoff` with a summary of:

- What was accomplished this session
- What the next session should focus on (reference memory files, don't duplicate content)
- Any flagged staleness or conflicts from Phase 3

</supporting-info>
