---
description: Review code against active CLAUDE.md atoms and architecture rules
allowed-tools: Read, Glob, Grep, Bash(git diff:*, git log:*, git show:*)
---

Review code against this project's active CLAUDE.md rules. Runs the code-reviewer skill.

## Usage

- `/review` — Review staged changes (or last commit if nothing staged)
- `/review lib/services/` — Review specific files or folder
- `/review HEAD~3..HEAD` — Review a git range
- `/review --pr` — Review all changes in current branch vs. main

## Process

1. **Detect scope** from the argument (files, diff range, or default to staged)
2. **Load active atoms** from this project's CLAUDE.md
3. **Run checks** against every active atom (base, stack, principles, project-specific)
4. **Report** findings by severity: 🔴 Critical, 🟡 Warning, 💡 Suggestion
5. **Offer fixes** — apply critical issues, fix all, or log patterns to project memory
