# Dream — Phase Details

Detailed instructions for each phase of the dream process. Referenced from [SKILL.md](SKILL.md).

---

## Phase 1 — Orient

- `ls` the memory directory to see what already exists
- Read `MEMORY.md` to understand the current index
- Skim existing topic files so you improve them rather than creating duplicates
- If `logs/` or `sessions/` subdirectories exist, review recent entries there

## Phase 2 — Gather Recent Signal

Look for new information worth persisting. Sources in rough priority order:

1. **Daily logs** (`logs/YYYY/MM/YYYY-MM-DD.md`) if present — these are the append-only stream
2. **Existing memories that drifted** — facts that contradict something you see in the codebase now
3. **Transcript search** — if you need specific context, grep the JSONL transcripts for narrow terms:

```bash
grep -rn "<narrow term>" ~/.claude/projects/<project>/ --include="*.jsonl" | tail -50
```

Don't exhaustively read transcripts. Look only for things you already suspect matter.

## Phase 3 — Consolidate

For each thing worth remembering, write or update a memory file at the top level of the memory directory. Follow the memory file format:

```markdown
---
name: {{memory name}}
description: {{one-line description — used to decide relevance in future conversations, so be specific}}
type: {{user, feedback, project, reference}}
---

{{memory content}}
```

### Memory types
- **user** — role, goals, preferences, knowledge
- **feedback** — guidance on approach (what to avoid, what to keep doing). Structure: rule, then **Why:** and **How to apply:**
- **project** — ongoing work, goals, initiatives, bugs. Structure: fact/decision, then **Why:** and **How to apply:**
- **reference** — pointers to external resources

### What NOT to save
- Code patterns, architecture, file paths (derivable from code)
- Git history (use git log)
- Debugging solutions (fix is in the code)
- Anything in CLAUDE.md files
- Ephemeral task details

### Focus on
- Merging new signal into existing topic files rather than creating near-duplicates
- Converting relative dates ("yesterday", "last week") to absolute dates so they remain interpretable after time passes
- Deleting contradicted facts — if today's investigation disproves an old memory, fix it at the source

## Phase 4 — Prune and Index

Update `MEMORY.md` so it stays under 200 lines. It's an **index**, not a dump — link to memory files with one-line descriptions. Never write memory content directly into it.

- Remove pointers to memories that are now stale, wrong, or superseded
- Demote verbose entries: keep the gist in the index, move the detail into the topic file
- Add pointers to newly important memories
- Resolve contradictions — if two files disagree, fix the wrong one
