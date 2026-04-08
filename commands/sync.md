---
description: Sync updated atoms into existing project CLAUDE.md
allowed-tools: Read, Write, Edit, Glob, Grep
---

Sync the latest atom versions from the plugin into this project's CLAUDE.md and Claude Code configuration. Preserves project-specific rules.

## Process

1. **Read** the current CLAUDE.md
2. **Identify sections** that came from atoms (they have `---` separators and match atom headers)
3. **Read** the current atom versions from the plugin's references
4. **Diff** each atom section against the current plugin version
5. **Show changes** to the user:
   - "base atom: 2 lines added (context hygiene section)"
   - "solid atom: no changes"
   - "validation-loop atom: NEW — not in project yet, recommend adding"
   - "banned-patterns atom: NEW — not in project yet, recommend adding"
   - "flutter stack: 1 line removed (outdated codegen command)"
6. **Preserve** the Project-Specific section (including i18n config) untouched
7. **Wait for user approval**
8. **Replace** atom sections with latest versions, keep project-specific rules
9. **Sync hooks** — if `.claude/settings.json` exists, check if hooks match the latest stack template. Report any differences and offer to update.
10. **Sync agents** — if `.claude/agents/` exists, check if agent definitions match the latest templates. Report differences.
11. **Report** token count before/after, changes applied, new atoms available

If the CLAUDE.md doesn't appear to use the atom system (no `---` separators, no recognizable atom sections), recommend running /migrate first.
