---
name: make-it-readable
description: Reviews any text or markdown file for readability issues targeted at non-native English speakers. Use when user asks to review readability, simplify language, make a document clearer, or improve structure of any text or markdown file — including skill files, docs, READMEs, or written instructions.
---

# Make It Readable

Reviews files for readability issues with non-native English speakers as the north star: plain language, clear structure, no redundancy.

## Workflow

1. **Read** the target file
2. **Analyze** against the checklist and open judgment
3. **Present** numbered issues with severity (`critical` / `suggestion`)
4. **Ask**: "Apply all / Select by number (e.g. 1,3,5) / Skip"
5. **Batch-apply** the selected fixes in a single edit

## Checklist

### Language
- **Idiomatic language** — idioms, slang, culturally-specific expressions (e.g., "stay in the loop" → "review regularly", "blast radius" → "risk of unintended impact")
- **Jargon without context** — technical or domain-specific terms used without explanation on first use
- **Complex sentence structure** — long or nested sentences that require re-reading to parse
- **Passive voice overuse** — indirect phrasing that obscures who does what

### Visual Structure
- **Text chunks** — paragraphs longer than 3-4 lines that should be broken into lists or sub-sections
- **Missing visual variety** — walls of text where headers, tables, or bullet lists would help scanning
- **Buried key information** — important points hidden mid-paragraph instead of leading or listed

### Content
- **Redundancy** — the same information explained in more than one place; consolidate to a single location
- **Unnecessary filler** — sentences or phrases that add length without adding meaning

### Interactive Elements
- **Checkboxes in reference content** — replace with regular bullets; checkboxes imply individual action on shared or reference pages

### Formatting
- **Inconsistent heading levels** — heading hierarchy that skips levels or uses levels inconsistently (e.g., H2 followed by H4)
- **Mixed list styles** — switching between bullets and dashes, or ordered and unordered lists without reason
- **Misused emphasis** — bold or italic applied inconsistently, or used for decoration rather than meaning
- **Inconsistent terminology** — the same concept referred to by different names across the document

## Judgment Layer

Beyond the checklist, flag anything a non-native English speaker would likely:
- Need to re-read multiple times to understand
- Misinterpret due to ambiguity
- Miss because it's visually buried or structured as dense prose

## Issue Format

```
1. [critical] Line 12 — Idiomatic language: "stay in the loop" → suggest: "review regularly"
2. [critical] Lines 5–8 — Text chunk: 6-line paragraph; break into a list
3. [suggestion] Line 3 — Jargon: "CSO" used without explanation on first use
4. [suggestion] Lines 20–22 — Redundancy: same point already made in lines 9–10
```

## Applying Fixes

After presenting all issues, ask:
> "Apply all / Select by number (e.g. 1,3,5) / Skip"

Apply the selected fixes in a single batch edit. Do not re-run the analysis after applying.

**Constraint:** Fix how something is said — never what is being said. Preserve all existing meaning, examples, and context exactly. If a fix would require changing the substance, flag it as a comment instead of applying it.
