---
description: Generate a Product Requirements Prompt (PRP) from an INITIAL.md feature brief
argument-hint: [initial-md-path (optional, defaults to INITIAL.md)]
allowed-tools: Read, Write, Glob, Grep, Bash(git:*), Bash(find:*), Bash(wc:*)
---

Generate a PRP (Product Requirements Prompt) for the feature described in the INITIAL.md file.

## Process

1. **Read the brief.** Load `$ARGUMENTS` or `INITIAL.md` from project root. If it doesn't exist, create one from the template at `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/prp/INITIAL.md.template` and ask the user to fill it out.

2. **Research the codebase.**
   - Read CLAUDE.md to understand project rules and architecture
   - Explore the folder structure to identify patterns
   - Find existing code that relates to the feature (search for related entities, services, tests)
   - Identify dependencies and potential conflicts

3. **Generate the PRP.** Using the template at `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/prp/prp-base.md.template`, create a detailed PRP that includes:
   - Research summary (what you found in the codebase)
   - Existing patterns to reuse (with file paths)
   - Step-by-step implementation tasks (file-by-file, with validation per task)
   - Risks and mitigations
   - Final validation checklist

4. **Save to PRPs/ directory.** Write the PRP to `PRPs/{feature-name}.md`. Create the directory if it doesn't exist.

5. **Report.**
   ```
   PRP generated: PRPs/{feature-name}.md

   Tasks: {count}
   Phases: {count}
   Files affected: {count}

   Next: Review the PRP, then run /execute-prp PRPs/{feature-name}.md
   ```

## Rules

- Be specific. Every task must name the exact file, the action (create/modify/delete), and what to do.
- Include validation per task. The executor needs to know how to verify each step.
- Reference existing code. Don't reinvent patterns that already exist in the project.
- Keep tasks small. If a senior engineer can't complete a task in one focused session, break it down.
