---
name: 1-1-prep
description: >
  Prepare for a recurring 1-1 meeting or capture the post-meeting note. Reads the person's profile,
  past 1-1 notes, vault cross-references, and git history to produce an opinionated brief with
  prioritized topics and silence-killer questions. Offers a share-ahead summary the manager can
  send beforehand, turning prep into a two-way tool. Post-meeting mode writes the 1-1 note, updates
  the person's profile, and persists decisions to memory. Use when the user says "prep my 1-1 with
  [name]", "1-1 prep", "brief me for my 1-1", "just finished 1-1 with [name]", or equivalent
  in any language.
---

# 1-1 Prep

**Always respond to the user in their language.**

Prepare for a recurring 1-1 by pulling everything that matters — past notes, vault cross-references, git history, and pattern signals — into an opinionated brief. After the meeting, close the loop: capture the note, update the person's profile, persist decisions to memory.

## Expected Vault Structure

This skill expects an Obsidian-style vault. Adapt these default paths to your layout:

| Purpose | Default path | Description |
|---|---|---|
| People profiles | `People/{Name}.md` | Person's role, preferences, growth focus |
| 1-1 notes | `1-1/{Name}/*.md` | Dated notes per person |
| Inbox | `Inbox/` | Where prep docs land |
| User profile | `Meta/user-profile.md` | Your own role and context |

## When to Use

- **Pre-meeting (Mode A)**: "prep my 1-1 with [name]", "brief me for my 1-1"
- **Post-meeting (Mode B)**: "1-1 went well [recap]", "just finished 1-1 with [name]"

## Mode A — Pre-meeting

Gather context from the person's profile, past 1-1 notes, vault cross-references, and git history. Analyze patterns (carried-forward debt, topic energy, pivot detection). Synthesize into an opinionated brief scaled to the meeting's actual complexity, then offer a share-ahead message the manager can send to the report beforehand.

Save to: `Inbox/YYYY-MM-DD — Meeting Prep — 1-1 with {Name}.md`

See [PROCEDURE.md](PROCEDURE.md) for detailed steps and pattern analysis rules.

## Mode B — Post-meeting

Write the 1-1 note, update the person's profile with new tactical knowledge, save decisions to memory, and mark resolved carried-forward items.

Save to: `1-1/{Name}/YYYY-MM-DD.md`

See [PROCEDURE.md](PROCEDURE.md) for detailed steps.

## Templates

See [TEMPLATES.md](TEMPLATES.md) for the prep-doc template (Mode A) and 1-1 note template (Mode B).

## Error Handling

- **Person not found**: Ask the user to confirm the name. Offer to create a stub profile.
- **No git repo configured**: Ask once for the path, save as a reference memory.
- **Git CLI missing**: Skip git phase, lean on vault cross-references, note in the brief.
- **Multiple repos**: Pull from each.
- **First 1-1 (no past notes)**: Skip carried-forward analysis. Lean on profile, cross-refs, and git.

## Final Report

See [PROCEDURE.md](PROCEDURE.md) for the structured report format for both modes.
