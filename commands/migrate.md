---
description: Migrate monolithic CLAUDE.md to composable atom system
allowed-tools: Read, Write, Glob, Grep, Bash(mkdir:*)
---

Migrate this project's existing CLAUDE.md into the composable atom system. This is a non-destructive operation.

## Process

1. **Read** the existing CLAUDE.md
2. **Backup** the original to `CLAUDE.md.backup`
3. **Classify** each section — map content to atoms (base, stack, principles) vs. project-specific
4. **Show the mapping** to the user before making changes:
   - "These rules match the base atom" → will be replaced by atom version
   - "These rules match the flutter stack atom" → will be replaced by atom version
   - "These rules match SOLID/DDD/Clean Arch/SOA/Testing/Security/API Design/Observability/Validation Loop/Banned Patterns" → will be replaced by atom version
   - "These rules are project-specific (including i18n config)" → will be preserved in the Project-Specific section
   - "These rules appear to be bloat" → recommend removal, ask for confirmation
5. **Wait for user approval** before proceeding
6. **Compose** new CLAUDE.md from atoms + preserved project-specific rules
7. **Scaffold** `.claude/settings.json` hooks based on detected stack (read from plugin hooks references)
8. **Scaffold** `.claude/agents/` if project complexity warrants subagents
9. **Create** `.claude/commands/` if missing
10. **Create** `INITIAL.md` and `PRPs/` for PRP workflow
11. **Create** `examples/` directory if missing
12. **Create** `docs/project_notes/` if missing
13. **Report** what changed, token count before/after, new infrastructure added
