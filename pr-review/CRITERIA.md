# PR Review — Architecture Criteria & Report Format

Referenced by [SKILL.md](SKILL.md) for Agent #3 (architectural evaluation) and the final report.

---

## Architecture Evaluation Criteria

Agent #3 must evaluate these dimensions. For each, assign a rating:
- **OK** — no concerns
- **WARN** — minor issue or missed opportunity
- **ISSUE** — clear violation or problem

| Dimension | What to look for |
|-----------|-----------------|
| **Scriptable Data Only** | Are magic numbers, thresholds, or tuning values hardcoded in C# when they should live in a ScriptableObject? Balance data belongs in `Gameplay/Balancing/` SOs, not code. |
| **Switch Statements** | Are there large switch/case blocks that could be replaced with polymorphism, dictionaries, or data-driven lookups? |
| **Code Repetition** | Are there duplicated code blocks (3+ similar lines) that should be extracted into a shared method? |
| **Single Source for Logic Rules** | Is the same business rule expressed in multiple places? Logic rules should have exactly one authoritative source. |
| **Reuse of Existing Functionality** | Does the PR introduce new code for something that already exists in the codebase? Use codegraph to check. |
| **Coupling** | Does the new code create tight coupling between unrelated systems? Circular dependencies or inappropriate direct references? |
| **Debug Cleanup** | Leftover `Debug.Log`, `Debug.LogWarning`, `print()`, `TODO`, `HACK`, `FIXME`, or commented-out code? |
| **Excessive Conditionals** | Deeply nested if/else chains (3+ levels) or methods with too many conditional branches? |
| **Data Organization** | Are data structures well-organized? Fields grouped logically? Enums, constants, and configs in appropriate locations? |

---

## Report Format

```markdown
## PR Review: <PR title>

**PR**: #<number> | **Author**: <author> | **Files reviewed**: <count of reviewable files> / <total files changed>

### Summary
<2-3 sentence summary of what the PR does>

### Issues Found

#### Bugs & Logic (<count>)
- **[file.cs:L42]** <description>
  Fix: <short fix summary>

#### Convention Violations (<count>)
- **[file.cs:L15]** <description>
  Fix: <short fix summary>

### Architecture Scorecard

| Dimension | Rating | Notes |
|-----------|--------|-------|
| Scriptable Data Only | OK / WARN / ISSUE | <brief note if not OK> |
| Switch Statements | OK / WARN / ISSUE | ... |
| Code Repetition | OK / WARN / ISSUE | ... |
| Single Source for Logic Rules | OK / WARN / ISSUE | ... |
| Reuse of Existing | OK / WARN / ISSUE | ... |
| Coupling | OK / WARN / ISSUE | ... |
| Debug Cleanup | OK / WARN / ISSUE | ... |
| Excessive Conditionals | OK / WARN / ISSUE | ... |
| Data Organization | OK / WARN / ISSUE | ... |

### Architecture Notes
<Only if there are WARN or ISSUE ratings — expand on what's wrong and the suggested fix>
```

### No-issues Format

```markdown
## PR Review: <PR title>

**PR**: #<number> | **Author**: <author> | **Files reviewed**: <count>

No issues found. All architecture dimensions rated OK.
```
