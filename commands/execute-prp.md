---
description: Execute a Product Requirements Prompt (PRP) task by task with validation
argument-hint: <prp-file-path>
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

Execute the PRP (Product Requirements Prompt) at the specified path.

## Process

1. **Load the PRP.** Read the file at `$ARGUMENTS`. If no argument, look for the most recent file in `PRPs/`.

2. **Load CLAUDE.md.** Read the project's rules to enforce during implementation.

3. **Execute sequentially.** For each task in order:
   a. Announce: "Starting Task {n}: {title}"
   b. Execute the task exactly as described
   c. Run the task's validation check
   d. If validation passes: mark complete, move to next task
   e. If validation fails: fix and re-validate (max 2 attempts)
   f. If still failing after 2 attempts: STOP and report the issue to the user

4. **Run final validation.** After all tasks:
   - Run the full validation checklist from the PRP
   - Run the project's build/lint/test pipeline
   - Verify no scope creep (only planned files were touched)

5. **Report.**
   ```
   PRP Execution Complete: {feature-name}

   Tasks: {completed}/{total}
   Files created: {list}
   Files modified: {list}
   Tests: {pass/fail count}

   Failed tasks (if any):
     Task {n}: {issue description}
   ```

## Rules

- Follow the PRP exactly. Don't skip tasks. Don't combine tasks. Don't add tasks.
- Run validation after EVERY task, not just at the end.
- If blocked, stop and ask — don't guess or improvise around the plan.
- Respect CLAUDE.md rules throughout execution.
- Two failures on the same task = stop and report. Don't spiral.
