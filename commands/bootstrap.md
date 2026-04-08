---
description: Bootstrap project with CLAUDE.md, hooks, subagents, commands, and PRP workflow
argument-hint: [spec-file-path (optional)]
---

Run the bootstrapper skill to generate a complete Claude Code configuration for this project.

If $ARGUMENTS is provided, use spec upload mode — read the file at @$1 and analyze it to infer project configuration.

If no arguments, use interview mode — ask questions to determine the right configuration.

Generate: CLAUDE.md, .claude/settings.json (hooks), .claude/agents/ (subagents), .claude/commands/, INITIAL.md, PRPs/, examples/, and docs/project_notes/.
