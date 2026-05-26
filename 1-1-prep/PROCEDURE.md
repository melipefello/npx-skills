# 1-1 Prep — Detailed Procedure

Step-by-step instructions for both modes. Referenced from [SKILL.md](SKILL.md).

---

## Mode A — Pre-meeting (detailed steps)

### 1. Discovery (parallel reads)

- `Meta/user-profile.md`
- `People/{Name}.md` — read DEEPLY. A single line like "board games opener works" is worth more than a generic question bank. Look for `## 1-1 Tactics`, `## Current Growth Focus`, communication preferences, preferred openers.
- Glob `1-1/{Name}/*.md` and identify: latest substantive note (skip 0-byte files — they are a signal, not data), empty/missing note count in the last 3 sessions, most recent 2-3 substantive notes.
- Any 1-1 instruction guide or template files in the 1-1 directory for framework context.

### 2. Cross-vault grep

Search folders outside the 1-1 directory for the person's name — leadership notes, project docs, meeting notes, weekly updates, TODO files, archives. Context outside the 1-1 folder is often the strongest conversation hook.

### 3. Git history (if the person is a code author)

Don't trust stale 1-1 notes about current work. Check memory for repo paths; ask the user once if unknown, then save as a reference memory.

```bash
git -C {repo} log --all --author="{firstname}" --since="{last_substantive_1-1_date}" \
  --pretty="%h | %ad | %s" --date=short
git -C {repo} log --all --merges --author="{firstname}" --since="..." \
  --pretty="%h | %ad | %s" --date=short
```

Synthesize into themes: group by sprint windows, by commit prefixes (`ADD/FIX/IMP/REMOVE/CLEANUP`), by branch names in merges. Flag spec-driven workflow adoption, tools shipped, tech-debt removals, merged PR count. If person is not a code author, skip git and lean harder on vault cross-references.

### 4. Pattern analysis

From everything gathered, flag:

- **Carried-forward items with deferral counts.** Anything deferred 3+ times must be reframed or formally killed.
- **Topic energy.** Look at which topics generated real substance in past conversations (extended discussion, follow-up actions, the person drove the thread) vs. which fell flat (short answers, no follow-up, repeatedly deferred). Use this to inform topic selection — lead with what energizes them, repackage or drop what doesn't land.
- **Empty-note streaks.** 2+ consecutive empty/missing notes — but don't assume this is a problem. Cross-reference with git activity and vault mentions: an IC who's actively shipping and has short, quiet 1-1s is healthy and low-ceremony, not disengaged. Only surface as a concern if other signals (low git activity, no vault mentions, carried-forward buildup) corroborate.
- **Pivot detection.** If git shows different work than the last 1-1 note described, discard the stale topic list.
- **Live growth track.** If there's a concrete stated growth area, anchor it explicitly. Do NOT re-ask "what are your growth goals?"

### 5. Synthesize

Use the prep-doc template in [TEMPLATES.md](TEMPLATES.md). Be opinionated.

**Scale the prep to the situation.** If no patterns are flagged, the last 1-1 was recent and substantive, and the relationship is healthy: skip "Context Worth Naming", condense git themes to one paragraph, trim silence-killers to 3, and omit "Watch-fors." A tight 1-page brief respects both your time and the meeting's actual complexity.

### 6. Save

Save to `Inbox/YYYY-MM-DD — Meeting Prep — 1-1 with {Name}.md`.

### 7. Offer share-ahead and grilling

Offer two opt-in follow-ups (do NOT auto-invoke either):

1. **Share-ahead**: *"Want me to draft a quick heads-up message you can send [Name] before the meeting? 3 bullets — what you're thinking about discussing + an open slot for their topics."* Generate using the share-ahead template in [TEMPLATES.md](TEMPLATES.md). This turns the prep from a manager advantage into a collaborative tool — the report arrives prepared, can redirect, and doesn't feel ambushed by well-researched questions.

2. **Grilling**: *"Want me to /grill-me you for 5 minutes? I'd push on: (1) [specific stress-point], (2) [...], (3) [...]."*

---

## Mode B — Post-meeting (detailed steps)

### 1. Write the 1-1 note

Save to `1-1/{Name}/YYYY-MM-DD.md` using the note template in [TEMPLATES.md](TEMPLATES.md). Three bullets beats zero — even a sparse note breaks an empty-streak pattern.

### 2. Update the person's profile

At `People/{Name}.md`. If new info surfaced (preferred openers, growth focus, communication preferences, engagement patterns), add or extend:

- `## Current Growth Focus` for live growth tracks
- `## 1-1 Tactics` for opener tactics and what engages
- `## Notes` for one-off dated observations

Create sections that don't exist; append to ones that do (don't overwrite).

### 3. Save decisions to memory

For each stated decision in the recap: if it sets a precedent that could affect other reports, save as a project memory. One-off operational details go in the 1-1 note only.

### 4. Mark resolved carried-forward items

If a long-deferred item was addressed, mark it as **Resolved** in the note's "Carried Forward" table.

### 5. Roll forward

Update "Topics for Next 1-1" with what was discussed and what's now pending.

---

## Final Report Format

**Mode A (pre-meeting):**
```
Session Complete

Saved to vault (1):
- "Meeting Prep: 1-1 with {Name} — {date}" -> Inbox/ [meeting-prep, 1-1]

Data sources:
- {N} past 1-1 notes ({M} substantive, {E} empty/missing)
- {C} cross-references in non-1-1 vault folders
- {G} git commits since {last_1-1_date} ({P} merged PRs)

Flagged:
- {N} carried-forward items, {N} deferred 3+ times -> need reframing
- Session gap: {description, or "none — recent and healthy"}
- Pivot mismatch: {description, or "none"}
- Live growth track: {name or "none stated"}

Offered: share-ahead {yes/no}, grilling {yes/no}
```

**Mode B (post-meeting):**
```
Session Complete

Saved to vault (1):
- "1-1 Summary: {Name} — {date}" -> 1-1/{Name}/

Profile updated: {Name}.md — added/extended {section names}

Memory saved ({N}):
- {precedent} -> project memory

Carried-forward changes:
- {N} items resolved
- {N} items rolled forward to next 1-1
- {N} items flagged for formal kill next time
```
