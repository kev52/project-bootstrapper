---
name: project-memory
description: >
  This skill should be used when the user says "log a bug", "record a decision",
  "add to project notes", "what bugs have we seen", "what decisions did we make",
  or needs to read/write project memory files (bugs.md, decisions.md, key_facts.md).
version: 0.1.0
---

# Project Memory

Maintain living documentation in `docs/project_notes/` that compounds knowledge across sessions.

## Files

- **bugs.md** — Known issues with root cause and solution. Format: `### {Bug title}` with Description, Root Cause, Fix, Date.
- **decisions.md** — Architecture decisions with rationale. Format: `### {Decision title}` with Context, Decision, Rationale, Alternatives Considered, Date.
- **key_facts.md** — Important implementation details that prevent mistakes. Format: `### {Fact}` with Detail, Why It Matters, Date.

## Behavior

**When logging a new entry:**
1. Read the relevant file
2. Append the new entry at the top (newest first)
3. Use today's date
4. Keep entries concise — dense information, no fluff

**When querying:**
1. Read the relevant file(s)
2. Search for matching entries
3. Summarize findings

**When a bug is fixed or decision is reversed:**
1. Don't delete the old entry
2. Add an update note with date: `> Updated {date}: {what changed}`

## Promotion Rule

If the same bug or fact appears across 3+ projects, suggest promoting it to the CLAUDE.md stack atom or base atom. This prevents re-learning the same lesson.
