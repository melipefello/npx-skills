---
name: pr-review
description: Review a GitHub PR locally in chat with Unity/C# architecture evaluation. Use when the user wants to review a PR, says "pr-review", or wants architectural feedback on a pull request. Reports findings in chat — never posts comments to GitHub.
argument-hint: "PR URL, PR number, or target branch comparison (e.g. 'develop...feature/my-branch')"
---

# PR Review (Local)

Structured, multi-agent PR review that reports findings **locally in chat only** — never post comments to GitHub. Focused on reviewable file types and Unity/C# architectural quality.

## Argument

The user passes one of:
- A GitHub PR URL (e.g. `https://github.com/org/repo/pull/123`)
- A PR number (e.g. `123`)
- A branch comparison (e.g. `develop...feature/my-branch`)

If nothing is passed, run `gh pr list` and ask which PR to review.

## File Filter

**Only review** these file types: `.cs`, `.asset`, `.json`

**Skip entirely**: `.meta`, `.scene`, `.prefab`, `.unity`, `.anim`, `.controller`, `.physicsMaterial2D`, `.mat`, `.lighting`, `.renderTexture`, `.shadergraph`, `.shader`, `.csproj`, `.sln`, `.png`, `.jpg`, `.wav`, `.mp3`, `.fbx`, `.ttf`, `.otf`, `.sprite`, `.spriteatlas`, `.guiskin`

When fetching the diff, mentally discard all files that don't match the reviewable types.

## Workflow

### 1. Resolve the PR

- If a PR URL or number: extract PR number, run `gh pr view <number>` to confirm it's open and not a draft.
- If a branch comparison: run `git diff <from>...<to> -- '*.cs' '*.asset' '*.json'` to get the diff directly.
- If nothing provided: `gh pr list` and ask.

### 2. Gather Context (parallel)

Launch two subagents in parallel:

**Subagent A — Project guidance**: Find all CLAUDE.md and AGENTS.md files at repo root and in directories touched by the PR. Return the file paths.

**Subagent B — PR summary + filtered diff**: Run `gh pr view <number>` and `gh pr diff <number>`. Filter the diff to only `.cs`, `.asset`, `.json` files. Return a concise summary plus the filtered diff content.

### 3. Parallel Code Review (3 specialized agents)

Launch 3 parallel subagents. Each receives the filtered diff and project guidance file paths.

Each agent returns issues as a list with: file path + line number(s), description, short fix summary (1-2 sentences), and confidence (0-100).

| Agent | Focus |
|-------|-------|
| **#1 Bug & Logic scan** | Obvious bugs, logic errors, null refs, off-by-ones, race conditions, missing null checks. |
| **#2 CLAUDE.md compliance** | Check changes against CLAUDE.md coding conventions (naming, async suffix, var usage, etc.). |
| **#3 Architectural evaluation** | Evaluate the diff against the criteria in [CRITERIA.md](CRITERIA.md). |

### 4. Confidence Filter

Discard issues scoring below **70**. Group remaining issues by category.

### 5. Report in Chat

Format the report using the template in [CRITERIA.md](CRITERIA.md). Do NOT post anything to GitHub. If no issues found, use the short "no issues" format instead.
