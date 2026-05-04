---
description: Install plugin rules into .claude/rules/ (auto-detect user vs project scope)
allowed-tools: Bash
---

Install the web-search-toolkit rules into the correct `.claude/rules/` directory.

## Steps

1. Find the plugin's `rules-templates` directory. Search in these locations (in order):
   - `~/.claude/plugins/web-search-toolkit/rules-templates/`
   - `.claude/plugins/web-search-toolkit/rules-templates/`
   - Any path matching `**/web-search-toolkit/rules-templates/` under `~/.claude/`

2. Detect installation scope:
   - If the plugin path is under `~/.claude/` → user-level → target is `~/.claude/rules/`
   - Otherwise → project-level → target is `.claude/rules/` (relative to project root)

3. Create the target `rules/` directory if it doesn't exist.

4. Copy all `.md` files from `rules-templates/` to the target `rules/` directory. If a file already exists, skip it and report which files were skipped.

5. Report results: which files were installed, which were skipped (already exist), and the target directory.
