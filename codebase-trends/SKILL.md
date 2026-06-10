---
name: codebase-trends
description: Analyze git history across authors to find churn hotspots, convergence files, fix-after-ship cycles, god classes, and actionable refactoring targets. Outputs a styled HTML report + markdown mirror. Use when the user wants to analyze commit trends, find code hotspots, identify technical debt, audit codebase health, or asks for a trends analysis.
argument-hint: "Optional: author names, date range, file extensions (e.g. 'since March, authors: Alice Bob, *.ts')"
---

# Codebase Trends Analysis

Multi-phase git history analysis that identifies churn hotspots, cross-author convergence files, fix-after-ship patterns, and concrete refactoring targets. Produces a styled HTML dashboard + markdown mirror.

## Inputs

Gather these before starting. Ask the user for any that aren't provided:

| Input | Required | Default |
|-------|----------|---------|
| **Authors** | Yes | — (ask user) |
| **Since date** | Yes | — (ask user, e.g. "last 3 months") |
| **File extensions** | No | `*.cs` |
| **Scope directory** | No | entire repo |
| **Ticket prefix pattern** | No | auto-detect `[XX-123]` or `PROJ-123` |
| **Commit prefixes** | No | `ADD, FIX, IMP, REMOVE, CLEANUP, REF, PERF, TEST, Revert` |
| **Output directory** | No | `Docs/tech-backlog/` |

## Workflow

### Phase 1 — Per-Author Deep Analysis (parallel agents)

Spawn **one agent per author** in parallel. Each agent is self-contained — no cross-dependencies. Give each agent the prompt template from [REFERENCE.md](REFERENCE.md) § Agent Prompt, substituting the author name, date, and file extension.

Each agent returns:
- Total commit count (merge vs non-merge)
- Full commit list with dates and complete messages
- All source files changed with frequency counts
- Category breakdown (ADD/FIX/IMP/REMOVE/etc.)
- Ticket-referenced commits
- AI co-authored commit count
- Per-author observations and patterns

### Phase 2 — Cross-Cutting Overlap Analysis

After all agents return, run the git commands from [REFERENCE.md](REFERENCE.md) § Cross-Cutting Commands to find:
- Files touched by **ALL** authors
- Files touched by **each pair** of authors
- Overall commit type distribution

### Phase 3 — Trend Synthesis

Apply the heuristics from [REFERENCE.md](REFERENCE.md) § Trend Detection to identify:
- God classes (high-touch files touched by multiple authors)
- Fix-after-ship cycles (features where FIX commits exceed a threshold)
- Multi-pass tickets (same ticket ID in 2+ fix commits)
- Service bottlenecks (registration/config files touched by everyone)
- Dead code signals (REMOVE commits clustered in sessions)
- Monolith files (single file receiving changes from many features)

### Phase 4 — Recommendations

For each identified trend, produce a concrete recommendation with:
- Severity: **Critical** / **High** / **Medium**
- The specific file(s) and their touch counts
- What to extract or decompose
- Target state after improvement

### Phase 5 — Output Generation

Generate two files using the structure in [REPORT-SPEC.md](REPORT-SPEC.md):
1. **HTML report** — dark-themed dashboard with metrics cards, TOC, tables, severity-coded recommendation cards
2. **Markdown mirror** — verbatim content of the report in `.md` format

Write both to the output directory. Open the HTML in the browser.

## Critical Implementation Notes

- **Parallel agents are essential.** A single-threaded approach will overflow context with git log output from multiple authors. Each agent handles one author's data in isolation.
- **Agent prompts must request FULL commit messages** (not truncated). `%s` is the subject line only — agents must use `git show --no-patch --format="%B" <hash>` when the full body matters.
- **Escape special characters in paths.** Directories with `!`, spaces, or other shell-special characters need `:(literal)` pathspec or quoting.
- **The cross-cutting analysis runs AFTER agents return.** It uses lightweight `comm`/`sort -u` commands, not heavy git log queries.
- **Recommendations must name specific files and counts.** Generic advice like "refactor large files" is useless — the value is in "decompose GearSpawnerController.cs (32 touches across 3 authors)."

See [REFERENCE.md](REFERENCE.md) for agent prompts, git commands, and trend heuristics.
See [REPORT-SPEC.md](REPORT-SPEC.md) for HTML/MD output structure and styling.
