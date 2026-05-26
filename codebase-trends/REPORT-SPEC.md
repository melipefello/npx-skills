# Report Output Specification

Both the HTML and Markdown reports share identical content. Generate them in parallel at the end.

## File Naming

```
{output_dir}/codebase-trends-{since_date}.html
{output_dir}/codebase-trends-{since_date}.md
```

Where `{since_date}` is the start of the analysis window in `YYYY-MM-DD` format.

After writing both files, open the HTML in the default browser:
- macOS: `open "path/to/file.html"`
- Windows: `start "" "path/to/file.html"`
- Linux: `xdg-open "path/to/file.html"`

## Report Sections

### Header
- Title: "Codebase Trends Analysis: {Project Name}"
- Subtitle: date range, author names
- Metrics dashboard: 4-6 top-level cards showing key numbers

### Metrics Dashboard Cards (pick the most relevant 4-6)
- Total commits
- Unique source files touched
- Files touched by all authors
- Bug fix ratio (FIX / total)
- AB tests / config registrations introduced
- Dead code LOC removed (if applicable)

### Table of Contents
Clickable links to each section. Two-column layout on wide screens.

### Section 1: Scope
Table with per-author commit counts (total, touching source files, unique files).

### Section 2: Convergence Files
Table of files touched by all authors. Columns: File, per-author touch counts, Total, Role description. Include a note about why these matter (merge conflicts, regression risk).

### Section 3: Per-Author Hotspot Files
One sub-section per author with a table: Touches | File | Why. Top 5 files per author.

### Section 4: Commit Category Breakdown
Table with categories as rows, authors as columns, plus Total and %. Include a key observation paragraph about the FIX ratio.

### Section 5: Trend Analysis (Critical Findings)
One card per trend detected. Each card has:
- Title (e.g., "TREND 1: The God Classes Problem")
- Description paragraph
- Supporting data (file names, touch counts, commit references)
- Evidence of impact (bug counts, fix cycles)

Card styling indicates severity via left border color.

### Section 6: Cross-Author Collaboration Patterns
List overlapping files per author pair. Note each author's primary domain. Identify coordination risk zones.

### Section 7: Major Feature Arcs
Table: Feature | Author | Dates | Commits | Files Touched | Post-Ship Bugs. Highlight rows with high bug counts.

### Section 8: Actionable Recommendations
Grouped by severity tier. Each recommendation is a card with:
- Task number and title (including the specific file name)
- Touch count and which authors are affected
- Current state / problem description
- Target state after improvement
- Files involved

### Section 9: Summary Metrics
Final reference table with all key metrics in one place.

### Footer
Generation date, analysis period, project name.

## HTML Styling Specification

Dark theme, GitHub-inspired. The style block should include:

### Color Palette (CSS variables)
```css
--bg: #0d1117;
--surface: #161b22;
--surface2: #1c2129;
--border: #30363d;
--text: #e6edf3;
--text-muted: #8b949e;
--accent: #58a6ff;        /* links, headings, metric values */
--accent2: #3fb950;       /* success / ADD badges */
--red: #f85149;           /* critical severity, FIX badges */
--orange: #d29922;        /* high severity */
--purple: #bc8cff;        /* h3 headings, IMP badges */
--pink: #f778ba;          /* inline code text */
```

### Component Styles

**Metric cards:** Grid layout (auto-fit, minmax 220px). Centered large number + small uppercase label.

**Tables:** Full width, collapsed borders, sticky `th` headers, hover highlight on rows. `th` uses `--surface2` background with `--accent` text.

**Severity cards:** `.card` base class with padding, border-radius 8px, surface background. Severity variants add a 4px left border:
- `.card-critical` — `var(--red)`
- `.card-high` — `var(--orange)`
- `.card-medium` — `var(--accent)`

**Badges:** Inline-block, pill-shaped (border-radius 12px), small uppercase text. Color-coded:
- `.badge-critical` — red bg/text
- `.badge-high` — orange bg/text
- `.badge-medium` — blue bg/text
- `.badge-add` — green bg/text
- `.badge-fix` — red bg/text
- `.badge-imp` — purple bg/text
- `.badge-remove` — muted bg/text

**Code spans:** Monospace font stack (Cascadia Code, Fira Code, JetBrains Mono), `--surface2` background, `--pink` text color, slight padding and border-radius.

**TOC:** Surface background card, two-column ordered list, accent-colored links.

### Responsive
- Below 768px: single-column TOC, 2-column metric grid, smaller table font
- Print media query: white background, dark text, light borders

### Typography
- Body: system font stack (-apple-system, BlinkMacSystemFont, Segoe UI, Helvetica, Arial)
- Line height: 1.6
- Max width: 1200px, centered
- h1: 2rem, border-bottom
- h2: 1.5rem, accent color, border-bottom
- h3: 1.15rem, purple color
- h4: 1rem, muted, uppercase, letter-spacing

## Markdown Mirror

The `.md` file is a verbatim content copy of the HTML — same sections, same tables, same text. Use standard GitHub-flavored markdown:
- Tables use pipe syntax
- Code uses backtick spans
- Severity labels use **bold** text instead of HTML badges
- Cards become `###` sub-headings
- No HTML tags in the markdown version
