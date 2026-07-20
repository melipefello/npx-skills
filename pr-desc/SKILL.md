---
name: pr-desc
description: Generate a PR description for the current branch. Asks the author for the branch goal first, then reads the diff. Optional "draft" argument also creates the draft PR on GitHub. Triggered by /pr-desc [draft].
argument-hint: "[draft]"
---

# PR Description

Generate a PR description for the current branch targeting `develop` (or a base branch the user names).

## Rule #1 — ask the goal first

Before reading any diff, ask the user ONE question: "What was the goal of this branch?" (motivation, constraints, anything the diff cannot show). A diff alone produces wrong rationale — the intent must come from the author. Skip the question only if the goal was already stated in this conversation.

## Workflow

1. Ask the goal (Rule #1).
2. Gather branch facts:
   - `git log develop..HEAD --oneline`
   - `git diff develop...HEAD --stat`
   - Read key changed files only if commits + stat are not clear enough.
3. Write the description:
   - **Summary** — 2–3 sentences: the goal (from the user) + what was done
   - **Changes** — bullets grouped by area, plain language
   - **QA notes** — what changed and what it impacts; no step-by-step test cases
   - **Out of scope / deferred** — only if relevant
4. Print the description in one fenced markdown block as the final message, so the user can grab it with `/copy`.

## Draft mode (`draft` argument)

After generating the description, also create the draft PR:

1. `gh auth switch` to the **work** account (work repos are not visible to the personal account — the exact account names are listed in the private global `~/.claude/CLAUDE.md`).
2. Write the body to a file in the session scratchpad (never `/tmp` — path breaks on Windows).
3. `gh pr create --draft --base develop --title "<title>" --body-file <file>`
4. Verify with `gh pr view --json url,isDraft` and report the URL.
5. `gh auth switch` back to the personal account to restore the default.

Never mark the PR ready for review. Never post comments. The user reviews and publishes.
