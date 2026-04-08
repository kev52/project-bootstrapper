---
description: Generate a PR description from branch diff
allowed-tools: Bash(git:*, gh:*)
model: haiku
---

Get the branch context:
- Current branch: !`git branch --show-current`
- Commits on this branch: !`git log main..HEAD --oneline`
- Files changed: !`git diff main..HEAD --stat`
- Full diff: !`git diff main..HEAD`

Generate a PR description with:
1. **Title**: Under 70 characters, describes the change
2. **Summary**: 2-3 bullet points covering what changed and why
3. **Test plan**: How to verify this works

Create the PR: gh pr create --title "{title}" --body "{body}"
