# 1-1 Prep — Templates

Templates used by [SKILL.md](SKILL.md).

---

## Template — 1-1 Prep Document (Mode A output)

```markdown
---
type: meeting-prep
date: {{today}}
meeting-date: {{meeting date}}
meeting-title: "1-1 with {{Name}}"
person: "{{Name}}"
tags: [meeting-prep, 1-1, {{name-lower}}]
status: inbox
created: {{ISO timestamp}}
---

# Meeting Prep: 1-1 with {{Name}} — {{meeting date}}

## Meeting Context
- **Participant**: [[05-People/{{Name}}]] — {{direct report / peer / skip-level}}
- **Last recorded 1-1**: {{date}} ({{"substantive" or "brief / not captured"}})
- **Session gap**: {{describe — e.g., "3 short sessions in a row — check if there's something to dig into or if things are just running smoothly"}}

## Context Worth Naming
{{Only include if a pattern needs attention — deferral debt, pivot mismatch, or a shift in meeting energy. Skip entirely if the relationship is healthy and the prep is routine. Frame as observable data, not a diagnosis.}}

## What {{Name}} Has Actually Been Doing (from git, since {{last 1-1 date}})

{{Grouped by workstream/sprint. Each group with date range and 3-6 bullet themes. Highlight tools built, specs/plans committed, merged PRs.}}

### Headline patterns
{{2-5 sentences on observed patterns — spec-driven workflow adoption, tool-builder mode, tech-debt cleanup habit, commit hygiene, etc.}}

## Topics to Talk About (prioritized)

### 🔥 Opener — {{match person's profile preferences}}
{{Highest-engagement starter. If profile says "board games opener works", lead with that. Otherwise lead with a concrete recent technical decision or shipped piece of work.}}

### 🛠️ Mid-meeting — substance
{{3-5 concrete current-work topics, each with a specific question. Pull from git themes and recent commits.}}

### 🌱 Carried-forward — repackaged, do NOT ask cold
{{Anything deferred 3+ times. Provide the REFRAMED phrasing, not the dead one. If a long-deferred topic is now subsumed by a live growth track, anchor it there instead.}}

### 🧭 Strategic / career — light touch
{{Only if energy permits. Career direction, pod isolation, cross-pod ambitions.}}

## General Guiding Questions (silence-killer bench)

Organized by emotional register. Pick by the person's live energy in the moment.

**Their happy place — technical:**
- {{question}}
- {{question}}

**Their happy place — craft / curiosity:**
- {{question}}
- {{question}}

**Cross-pod / system:**
- {{question}}

**Manager-direction (use rarely, lands hard):**
- "What's one thing I'm doing that's making your work harder?"
- "If I disappeared for two weeks, what would break first?"

**Personal:**
- {{question matched to known interests, e.g. board games, sports, family}}

## Open Items from Past 1-1s

| Item | Last raised | Deferral count | Suggested action |
|---|---|---|---|
| {{item}} | {{date}} | {{N}} | {{repackage / kill / resolve}} |

## Tactical Tips for the Live Session

- **Opener**: {{specific opener that matches profile}}
- **Bridge into substance**: {{the natural transition — e.g., "board games → mechanics you'd steal → his own systems work"}}
- **Bring, don't just ask**: {{any open manager-owned action items — come with a proposal, not a bounce-back}}
- **Exit ramp**: if it's empty at minute 25, end early — "let's get five back, I'd rather have a great 1-1 next time" beats 10 minutes of silence.
- **Vocabulary match**: speak in their register (technical / craft / systems / etc.). Don't pivot to HR-speak — it kills the energy.
- **This prep is a launchpad, not a script.** The person across from you will know the difference between genuine curiosity and performed preparation. Use the context to be more present, not more rehearsed. If a question lands differently in the room than it read on paper, follow their energy — not your prep doc.

## Watch-fors
{{Things to stay attuned to during the conversation — unresolved threads from past sessions, topics they may circle back to, energy shifts worth exploring in the moment.}}

---

### Suggested next agent
- **Skill**: 1-1-prep (Mode B — post-meeting)
- **Reason**: capture the actual 1-1 note after the session

*Generated on {{today}}*
```

---

## Template — Share-ahead Message (Mode A optional output)

A lightweight heads-up the manager sends to the report before the meeting. Keep it casual and short — the goal is to give them a chance to prepare and redirect, not to dump your prep doc on them.

```markdown
Hey {{Name}}, quick heads-up for our 1-1 {{meeting day}} —

I was thinking we could talk about:
- {{Topic 1 — something concrete from their recent work, framed as curiosity not interrogation}}
- {{Topic 2 — a carried-forward item or decision that needs closure}}
- {{Topic 3 — optional: growth/career thread, only if there's a live one}}

Anything you want to add or swap in? Happy to go wherever's most useful for you.
```

Adapt the tone to match the relationship (more casual for established reports, slightly more structured for newer ones). Never include your internal pattern analysis, watch-fors, or manager notes in the share-ahead.

---

## Template — 1-1 Note (Mode B output)

Based on `02-Areas/Work/Work/1-1/1-1 Template.md`, augmented for post-meeting capture.

```markdown
# 1-1 Summary: {{Name}}

Date: {{date}}

Previous 1-1: {{previous date — note "(not captured)" if there's a gap}}

---

## Quick Status

|||
|---|---|
|Mood|{{}}|
|Workload|{{}}|
|Blockers|{{}}|

---

## How It Went

{{One paragraph honest read. Include opener used, energy level, what worked, what didn't.}}

---

## Topics Covered

### {{Topic 1}}
- {{}}

### {{Topic 2}}
- {{}}

### {{Decision: title}} {{(if any decisions were stated)}}
- **Decision**: {{}}
- **Memory-worthy?**: {{yes/no — if yes, save as project memory}}

---

## Action Items

|Action|Owner|Due|
|---|---|---|
|{{}}|{{}}|{{}}|

---

## Topics for Next 1-1

- [ ] {{}}

---

## Carried Forward

|Item|Status|
|---|---|
|{{old item}}|{{✅ Resolved / 🟡 Still pending / 🔴 Kill formally next time}}|

---

## Summary

{{One paragraph synthesis.}}

---

## Appendix: Manager Notes (Internal)

### Worked This Time
- {{}}

### Still Pending
- {{deferred items with counts}}

### Watch For
- {{}}
```
