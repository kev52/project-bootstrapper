---
description: Generate a conventional commit from staged changes
allowed-tools: Bash(git:*)
model: haiku
---

Review the staged changes: !`git diff --cached --stat`

Full diff: !`git diff --cached`

Generate a conventional commit message following these rules:
- Prefix: feat:, fix:, chore:, refactor:, test:, docs:
- Subject line under 72 characters, imperative mood
- Body explains WHY, not WHAT (the diff shows what)
- One logical change per commit

Present the commit message, then run: git commit -m "{message}"
