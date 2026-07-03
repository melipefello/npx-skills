---
name: wrap-session
description: "One-command end-of-session ritual — runs /dream (memory consolidation) then /end-session (documentation pass) in order, with no handoff by default. Use when the user says 'wrap up', 'wrap it up', 'that's all for this session', 'closing ritual', or chains /dream and /end-session in one request. If the user invokes /dream or /end-session individually, run that skill alone instead."
---

# Wrap Session

Run the end-of-session ritual as one command. This replaces the manual checklist of typing `/dream`, `/end-session`, and a sync-validation step as separate prompts (stacked slash commands in one prompt get swallowed as arguments of the first — never rely on that).

## Arguments

| Command | Behavior |
|---|---|
| `/wrap-session` | dream (project scope) → end-session, **no handoff** |
| `/wrap-session handoff` | same, but end-session does produce the handoff |
| `/wrap-session user` / `all` | pass the scope through to dream |

## Steps — in this order

1. **Invoke the `dream` skill** with the scope argument (default: project). Dream's own early-exit check applies: a no-op dream is a fast, correct outcome — do not force a full pass.
   - Composition note: skip dream's final "resync to repo" phase; the resync runs once, at the end of end-session, instead of twice.
2. **Invoke the `end-session` skill.** Run all of its phases, including its memory-resync step, but **do not invoke `/handoff`** unless the `handoff` argument was given — the wrap-up ritual defaults to no handoff.
3. **Do not add a separate sync-validation step.** The resync inside end-session already heals and reports the memory mirror (and repos that automate mirror sync with hooks make it a fast no-op). If it reports conflicts or orphans, surface them to the user verbatim.

## Output

End with a combined summary, at most ~10 lines:

```
## Session wrapped
- Dream: [consolidated/updated/pruned counts, or "no-op — nothing new"]
- Memories: [files written/updated this pass]
- Mirror sync: [clean / healed N / CONFLICT details]
- Open follow-ups: [from end-session phase 5, or "none"]
```

Do NOT commit or push anything unless the user asks.
