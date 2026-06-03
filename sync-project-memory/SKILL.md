---
name: sync-project-memory
description: Hardlink the current project's auto-memory files (~/.claude/projects/<slug>/memory/*.md) into a Memory/ folder at the project root so they can be tracked in git and edited from either location with bidirectional sync. Use when the user asks to "sync memory", "link project memory into repo", "track memory in git", "clone memory to root", "expose memory files", or any variation that wants the per-project memory directory mirrored into the working tree without copying.
---

# sync-project-memory

Hardlink every `.md` file in the current project's auto-memory folder into `<project-root>/Memory/`. Hardlinks share one inode across both paths — edits in either location mutate the same file. Git tracks the project-root copy as a normal file.

## Quick start

1. Verify cwd is the project root (where you want `Memory/` to live).
2. Derive the memory-folder slug from the absolute cwd: replace every `:` and every `\` (or `/` on Unix) with `-`. Example: `C:\Root\Experimental\SteamBallGameDocumentation` → `C--Root-Experimental-SteamBallGameDocumentation`.
3. Memory source path: `~/.claude/projects/<slug>/memory/`. On Windows: `C:\Users\<user>\.claude\projects\<slug>\memory\`.
4. If the memory source folder does not exist, stop and tell the user — there is nothing to sync.
5. Create `<project-root>/Memory/` if missing.
6. For each `.md` in the source folder, create a hardlink at `<project-root>/Memory/<filename>`. Skip files that are already hardlinked to the same inode (idempotent re-run).
7. Verify with `fsutil hardlink list` (Windows) or `stat` (Unix) that the link count ≥ 2 and the sizes match the source.
8. Report: source path, destination path, number of files linked / skipped / errored.

## Cross-volume rule

Hardlinks require both paths on the same filesystem volume. If the user's home drive differs from the project drive, **abort with a clear error** — do not silently fall back to copy, symlink, or junction. The whole point is bidirectional sync; a copy would drift, a symlink may need admin/dev mode on Windows, a junction changes the per-file semantics. Tell the user the volume mismatch and stop.

## Platform commands

**Windows (PowerShell)** — preferred when on Windows:

```powershell
$cwd = (Get-Location).Path
$slug = $cwd -replace '[:\\]', '-'
$src  = Join-Path $env:USERPROFILE ".claude\projects\$slug\memory"
$dst  = Join-Path $cwd "Memory"

if (-not (Test-Path $src)) { throw "no memory folder at $src" }
if ((Get-Item $src).PSDrive.Name -ne (Get-Item $cwd).PSDrive.Name) {
  throw "cross-volume: src=$($src) dst=$($dst) — hardlinks require same volume. Aborting."
}

New-Item -ItemType Directory -Force $dst | Out-Null
Get-ChildItem $src -Filter *.md | ForEach-Object {
  $target = Join-Path $dst $_.Name
  if (Test-Path $target) {
    # already exists — verify same inode via fsutil; skip if so
    return
  }
  New-Item -ItemType HardLink -Path $target -Target $_.FullName | Out-Null
  "linked: $($_.Name)"
}
```

**Unix (bash)** — for macOS / Linux:

```bash
cwd="$(pwd)"
slug="$(printf '%s' "$cwd" | tr '/:' '--')"
src="$HOME/.claude/projects/$slug/memory"
dst="$cwd/Memory"

[ -d "$src" ] || { echo "no memory folder at $src"; exit 1; }

# same-filesystem check via device id
src_dev="$(stat -c %d "$src" 2>/dev/null || stat -f %d "$src")"
dst_parent_dev="$(stat -c %d "$cwd" 2>/dev/null || stat -f %d "$cwd")"
[ "$src_dev" = "$dst_parent_dev" ] || { echo "cross-volume — aborting"; exit 1; }

mkdir -p "$dst"
for f in "$src"/*.md; do
  [ -e "$f" ] || continue
  name="$(basename "$f")"
  target="$dst/$name"
  [ -e "$target" ] && continue
  ln "$f" "$target" && echo "linked: $name"
done
```

## Verification

After linking, sanity-check one file:

- Windows: `fsutil hardlink list <Memory/<file>>` should list ≥ 2 paths.
- Unix: `stat -c %h <Memory/<file>>` should print ≥ 2 (link count).

If link count is 1, the file is not a hardlink — investigate (did the OS silently fall back to copy?).

## Edge cases

- **Re-run is idempotent.** If `Memory/<file>` already exists with the same inode as the source, skip silently. If it exists with a different inode, warn — the link broke at some point (likely an editor that delete-then-recreates).
- **New memory files added after first sync** — re-running the skill picks them up (only `.md` files at the top level of the memory folder; sub-directories are not recursed in v1).
- **File deleted from source after sync** — the project-root copy survives (a hardlink keeps the inode alive until all paths are removed). Re-running the skill does not clean up orphans. Tell the user to `rm Memory/<orphan>` manually if they want to mirror deletions.
- **Memory folder empty** — create `Memory/` and a `.gitkeep` so the folder commits empty rather than failing.
- **Non-`.md` files in source** — ignored in v1 (memory is markdown-only by convention).

## Git considerations

The skill itself does **not** commit. After it runs, the project root has a new `Memory/` directory with tracked-ready files. The user (or a follow-up step) decides whether to `git add Memory/` and commit. If the repo has a `.gitignore` that excludes `Memory/`, the skill should warn the user.

## What this skill does not do

- Does not push memory files anywhere remote on its own.
- Does not encrypt / redact memory contents — the user must review for secrets before committing to a public repo.
- Does not handle the case where the project memory lives outside `~/.claude/projects/<slug>/memory/` (custom Claude Code config). If that comes up, prompt the user for the source path.
- Does not auto-resolve cross-volume scenarios. Aborts with an error per the cross-volume rule above.
