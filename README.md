# npx-skills

Personal collection of [Claude Code agent skills](https://docs.claude.com/en/docs/claude-code/skills). Each folder is one skill: a `SKILL.md` with frontmatter (name, description, triggers) plus optional support files loaded on demand.

## Install

Copy or link a skill folder into:

- `~/.claude/skills/` — available in every project
- `<project>/.claude/skills/` — available in one project

## Skills

### Planning workflow

| Skill | What it does |
|---|---|
| [grill-me](grill-me/SKILL.md) | Entry point: runs `grilling` with `domain-modeling`, then offers a spec write-up (`Docs/<Feature>/<Feature>Spec.md`). |
| [grilling](grilling/SKILL.md) | Core relentless interview — one question at a time, facts looked up, decisions asked. Building block for grill-me. |
| [domain-modeling](domain-modeling/SKILL.md) | Maintains `CONTEXT.md` (glossary + dated decisions log) and `Docs/ADR/` during design sessions. |
| [zoom-out](zoom-out/SKILL.md) | Ask the agent for a higher-level map of modules and callers. User-invoked only. |

Typical flow: discuss → `/grill-me` → implement.

Upstream tracking: `grilling` and `domain-modeling` are forks of [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/grilling`, `skills/engineering/domain-modeling`) with small local edits — catch up by diffing each file against its upstream twin. `grill-me` and `SPEC-FORMAT.md` are local-only. `to-spec` and `to-plan` were removed 2026-07 after a usage audit found zero uses in 45 days.

### Memory and session lifecycle

| Skill | What it does |
|---|---|
| [wrap-session](wrap-session/SKILL.md) | One-command closing ritual: `dream` then `end-session`, no handoff by default. |
| [dream](dream/SKILL.md) | Consolidate memory files: merge, dedupe, prune, keep the index under 200 lines. Early-exits when nothing new happened since the last consolidation. |
| [end-session](end-session/SKILL.md) | End-of-session pass: capture decisions in memory, review staleness, produce a handoff. |
| [handoff](handoff/SKILL.md) | Compact the conversation into a handoff document for the next session. |
| [sync-project-memory](sync-project-memory/SKILL.md) | Hardlink the project's auto-memory files into `<repo>/Memory/` so git can track them. Repos opt in by running it once; `dream` and `end-session` then keep the links fresh. |

### Code review and analysis

| Skill | What it does |
|---|---|
| [pr-desc](pr-desc/SKILL.md) | Generate a PR description for the current branch from the author's stated goal + the diff; `draft` also creates the draft PR. |
| [pr-review](pr-review/SKILL.md) | Local Unity/C# PR review with architecture scorecard. Reports in chat only — never posts to GitHub. |
| [codebase-trends](codebase-trends/SKILL.md) | Git-history analysis across authors: churn hotspots, god classes, fix-after-ship cycles. Outputs an HTML report + markdown mirror. |

### People

| Skill | What it does |
|---|---|
| [1-1-prep](1-1-prep/SKILL.md) | Prep a 1-1 meeting from vault notes + git history, or capture the post-meeting note. Expects an Obsidian-style vault. |

## Conventions

- Lean `SKILL.md`, details in support files (`PROCEDURE.md`, `REFERENCE.md`, `TEMPLATES.md`, ...) loaded only when needed.
- Skills that write docs use `CONTEXT.md` (domain glossary) and `Docs/ADR/` (decision records) as shared conventions.
- Memory files use the Claude Code format: frontmatter with `name`, `description`, and `type` nested under `metadata:`.
