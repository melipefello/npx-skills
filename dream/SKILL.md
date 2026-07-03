---
name: dream
description: "Memory consolidation — performs a reflective pass over Claude's memory files, synthesizing recent session learnings into organized, deduplicated memories with line-limit enforcement. Use when the user says 'dream', 'consolidate memory', 'clean up memory', 'organize memories', 'memory maintenance', or any request to tidy/consolidate/optimize Claude's memory files. Takes priority over similar plugin skills (e.g. consolidate-memory) when both match."
---

# Dream: Memory Consolidation

You are performing a dream — a reflective pass over your memory files. Synthesize what you've learned recently into durable, well-organized memories so that future sessions can orient quickly.

## Targeting

The user can specify a scope. Parse the argument after `/dream`:

| Command | Target |
|---|---|
| `/dream` (no args) | Current project memory only |
| `/dream user` | User-level memory only |
| `/dream all` | Both user-level AND current project memory (run phases on each separately, report combined results) |

## Locating the Memory Directory

List `~/.claude/projects/` and match the current working directory to the project folder name (dashes replace path separators). The memory directory is at:
```
~/.claude/projects/<matched-project>/memory/
```

For **user-level memory**, find the entry in `~/.claude/projects/` that matches just the user's home directory (e.g., `C--Users-YourName`). If ambiguous, ask the user.

If no matching memory directory exists (or it contains no memory files), there is nothing to consolidate — tell the user and stop. Do not create an empty memory structure.

Session transcripts (JSONL files) are in the parent directory of the memory folder. **Index max lines:** 200.

## Phases

Work through phases 0-5 in order. See [PHASES.md](PHASES.md) for detailed instructions on phases 1-4.

0. **Early-exit check** — before reading anything else, decide if there is new signal at all:
   - Find when the last consolidation happened: the newest modification time across the memory files (or, if the repo mirrors memory into git, the last commit touching the memory index).
   - List session transcripts (JSONL, in the parent directory of the memory folder) modified after that time, excluding the current session's own transcript.
   - If there are none, and the current session itself contains no consolidatable work (e.g. it only ran wrap-up commands), report `Dream: no-op — last consolidation <time>, no new sessions since` and STOP. Do not read every memory file, do not parse transcripts, do not verify mirror links.
1. **Orient** — read the memory directory and existing index
2. **Gather Recent Signal** — find new information worth persisting
3. **Consolidate** — write or update memory files with proper frontmatter
4. **Prune and Index** — update MEMORY.md, remove stale entries, stay under 200 lines
5. **Resync to repo** (project scope only) — if the repo root already has a `Memory/` folder, invoke `/sync-project-memory` to re-link the project memory into it, repairing hardlinks broken by file writes and tracking newly-created files. Skip if there is no `Memory/` folder (the repo has not opted in) and for `/dream user` (user-level memory isn't a project repo).

## Output

Return a brief summary:

```
## Dream Complete — [scope: project / user / all]

### Consolidated
- [what was merged/created]

### Updated
- [what was modified]

### Pruned
- [what was removed or cleaned up]

### No Change
- [if memories are already tight, say so]
```
