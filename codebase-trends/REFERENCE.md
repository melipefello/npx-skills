# Codebase Trends — Reference

## Agent Prompt Template

Spawn one agent per author. Replace `{{AUTHOR}}`, `{{SINCE_DATE}}`, `{{FILE_EXT}}`, `{{SCOPE_DIR}}`, `{{TICKET_PATTERN}}`, and `{{COMMIT_PREFIXES}}` with the user's inputs.

> **Important:** Each agent is self-contained. Do NOT share data between agents. Each runs its own git queries and returns a structured report.

```
I need a thorough analysis of all commits by {{AUTHOR}} since {{SINCE_DATE}} in this git repository. This is for a codebase trends analysis.

Do these steps:

1. Get the TOTAL count of all commits across all branches:
   - `git log --all --author="{{AUTHOR}}" --since="{{SINCE_DATE}}" --oneline | wc -l` (total)
   - `git log --all --author="{{AUTHOR}}" --since="{{SINCE_DATE}}" --oneline --no-merges | wc -l` (non-merge)

2. Get all commit hashes, dates, and FULL messages (not truncated) for ALL commits since {{SINCE_DATE}} across all branches. Use `--format="%H|%an|%ad|%s" --date=short` first for the overview, then `git show --no-patch --format="%B" <hash>` for any commits where you need the full body.

3. For EACH commit that touches {{FILE_EXT}} files{{SCOPE_DIR_CLAUSE}}, list all changed files with their change type using:
   `git show --name-status --format="" <hash> -- '{{FILE_EXT}}'`
   The change types are: A (added), M (modified), D (deleted).

4. Create a frequency count: which source files are most frequently changed? Sort descending.

5. Categorize each commit by its prefix. The recognized prefixes are: {{COMMIT_PREFIXES}}.
   Parse the subject line — strip any ticket prefix like [XX-123] first, then match the prefix.
   Commits that don't match a known prefix go into OTHER.

6. List all commits that reference ticket numbers. Look for patterns like:
   {{TICKET_PATTERN}}

7. Check for AI co-authored commits by looking for "Co-Authored-By:" or "Made-with:" in commit bodies.

Report back with ALL of the following (do not truncate or abbreviate):
- Total commit count (merge + non-merge breakdown)
- Full list of ALL non-merge commits with dates and complete subject lines, grouped by month or week
- List of ALL source files changed with frequency counts (how many commits touch each file), top 30+ minimum
- Breakdown by category (ADD/FIX/IMP/REMOVE/etc.) with counts and percentages
- All ticket-referenced commits with ticket ID and commit message
- AI co-authored commit count and tools used
- Your observations: busiest days/weeks, major feature arcs, files that keep getting fixed, work domain patterns

Be thorough — I need every detail. Do not summarize or abbreviate the commit list.
```

Where `{{SCOPE_DIR_CLAUSE}}` is either empty or ` under {{SCOPE_DIR}}`.

## Cross-Cutting Git Commands

Run these AFTER all per-author agents have returned. These are lightweight commands.

### Files touched by all N authors

For each author, build a sorted unique file list, then intersect:

```bash
# Build per-author unique file lists (sed strips the blank lines --format="" emits per commit,
# which would otherwise survive sort -u and show up as a phantom entry in every intersection)
files_A=$(git log --all --author="Author_A" --since="DATE" --format="" --name-only -- '*.ext' | sed '/^$/d' | sort -u)
files_B=$(git log --all --author="Author_B" --since="DATE" --format="" --name-only -- '*.ext' | sed '/^$/d' | sort -u)
files_C=$(git log --all --author="Author_C" --since="DATE" --format="" --name-only -- '*.ext' | sed '/^$/d' | sort -u)

# Intersect all (3 authors)
echo "=== Files touched by ALL 3 authors ==="
comm -12 <(comm -12 <(echo "$files_A") <(echo "$files_B")) <(echo "$files_C")

# Pairwise overlaps (excluding the third)
echo "=== A + B only ==="
comm -12 <(echo "$files_A") <(echo "$files_B") | comm -23 - <(echo "$files_C")

echo "=== A + C only ==="
comm -12 <(echo "$files_A") <(echo "$files_C") | comm -23 - <(echo "$files_B")

echo "=== B + C only ==="
comm -12 <(echo "$files_B") <(echo "$files_C") | comm -23 - <(echo "$files_A")
```

For 2 authors, simplify to a single `comm -12`. For 4+, nest the `comm` calls or use an `awk` approach.

### Per-author touch counts on convergence files

For each convergence file found above, count how many commits each author made touching it. This data goes into the convergence file table.

```bash
git log --all --author="AUTHOR" --since="DATE" --oneline -- "path/to/file.ext" | wc -l
```

## Trend Detection Heuristics

Apply these rules to the aggregated data. Thresholds are defaults — adjust if the dataset is unusually large or small.

### 1. God Classes

**Signal:** A single file with **15+ total touches** across all authors, OR touched by **all authors** regardless of count.

**Action:** Recommend decomposition. Name the file, its total touches, per-author breakdown, and the distinct responsibilities it handles (infer from commit messages).

### 2. Fix-After-Ship Cycles

**Signal:** A feature arc (group of commits with related subjects over a date range) where the ratio of FIX commits to ADD commits is **> 0.5** (more than one fix per two features).

**How to detect:** Group commits by feature name (inferred from commit subject keywords, ticket IDs, or file clusters). Count ADD and FIX commits per group.

**Action:** Name the feature, its ADD and FIX counts, the files involved, and the types of bugs (state management, null refs, off-by-one, etc.). Recommend a pattern to prevent recurrence.

### 3. Multi-Pass Tickets

**Signal:** The same ticket ID (e.g., `[GF-109]`) appears in **2+ FIX commits**, OR a ticket has a FIX followed by a Revert.

**Action:** List each multi-pass ticket with its attempts, the root cause of each attempt, and whether the final fix held. Look for common root causes across tickets.

### 4. Service/Config Bottlenecks

**Signal:** A file touched by **all authors** whose commit messages reference "register", "add service", "add config", or similar. Typically a service locator, DI container registration, or config registry.

**Action:** Recommend moving toward self-registration or attribute-based discovery to reduce the central bottleneck.

### 5. Dead Code Signals

**Signal:** A cluster of **5+ REMOVE commits within a single day** from one author, especially if the commits mention "dead", "orphan", "unused", "stale", or "commented-out".

**Action:** Note the scope of the cleanup (LOC removed, files deleted). Recommend regular audits to prevent accumulation. Flag if actual bugs were discovered during cleanup.

### 6. Monolith Services

**Signal:** A file touched by **3+ distinct features** (inferred from commit message topics) with **10+ touches** total. Typically analytics services, event dispatchers, or data managers.

**Action:** Recommend domain-specific decomposition. Name the domains that currently coexist in the file.

### 7. Initialization / Ordering Fragility

**Signal:** Commits that mention "initialization order", "service init", "depends on", "must come before/after", or files that are context/bootstrap roots touched by all authors.

**Action:** Recommend dependency-declared initialization over manual ordering.

## Commit Prefix Defaults

These are the default recognized prefixes. Override with user input if their project uses different conventions.

| Prefix | Meaning |
|--------|---------|
| `ADD` | New feature or capability |
| `FIX` | Bug fix |
| `IMP` | Improvement to existing feature |
| `REMOVE` | Removal of dead code, obsolete features |
| `CLEANUP` | Code style, formatting, refactoring |
| `REF` | Refactoring (structural change, no behavior change) |
| `PERF` | Performance improvement |
| `TEST` | Test-related changes |
| `Revert` | Revert of a previous commit |

Commits not matching any prefix: categorize as **OTHER**, then sub-categorize (infra, config, CI, docs, WIP, merge, version bumps).

## Ticket Pattern Defaults

Auto-detect by scanning the first 20 commit subjects for bracketed patterns:

```
\[([A-Z]{1,10}-\d+)\]    — matches [GF-123], [PROJ-45], [AB-1]
([A-Z]{1,10}-\d+)         — matches PROJ-123 without brackets
#(\d+)                     — matches #123 (GitHub issues)
```

Use whichever pattern appears most frequently. If none found, skip ticket analysis.
