---
name: auditor
description: >
  This skill should be used when the user says "audit my CLAUDE.md",
  "review my project config", "is my CLAUDE.md bloated", "check my setup",
  "improve my CLAUDE.md", "what's missing in my config", or wants to
  compare their project's AI configuration against the atom templates.
version: 1.0.0
---

# Project Auditor

Analyze an existing project's Claude Code configuration against the atom library. Produce a health report with specific, actionable fixes.

## Process

### Step 1: Read Current State

Read the project's CLAUDE.md from the project root. If none exists, recommend running `/bootstrap` instead and stop.

Also check for:
- `backbone.yml` (project structure map)
- `.claude/settings.json` (Claude Code hooks)
- `.claude/agents/` (subagent definitions)
- `.claude/commands/` (slash commands)
- `.claude/skills/` (custom skills)
- `.claude/rules/` (path-scoped rules)
- `INITIAL.md` (PRP feature brief template)
- `PRPs/` (PRP output directory)
- `examples/` (code example patterns)
- `docs/project_notes/` (project memory)
- `docs/project_notes/review_patterns.md` (recurring review findings)
- `CLAUDE.local.md` (personal config)
- `mcp.json` or `.mcp/` (MCP server configuration)

### Step 2: Token Analysis

Count the words in CLAUDE.md. Estimate tokens (words * 4/3).

Report:
- Current token count vs. 2,500 budget
- If over budget: flag as CRITICAL
- If 2,000-2,500: flag as WARNING
- If under 2,000: flag as HEALTHY

### Step 3: Atom Coverage Audit

Read the atom files from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/atoms/`.

For each atom category, check if the project's CLAUDE.md covers it:

**Base rules** — Check for presence of:
- Tradeoff-before-code directive
- Scope enforcement ("stay in scope")
- Type strictness rule
- Error handling rule
- String externalization rule
- Context hygiene section (one task per session, /clear, compact at 50%)
- Verification checklist

**Stack rules** — Detect the stack from build commands or framework mentions, then check:
- Are the correct build/test/lint commands present?
- Are stack-specific patterns documented?
- Are verification steps stack-appropriate?

**Principles** — For each principle the project appears to use (infer from folder structure, imports, patterns):
- SOLID: look for interface patterns, dependency injection
- DDD: look for domain/, entities/, value_objects/ folders
- Clean Architecture: look for layered folder structure
- SOA: look for services/, api/, gateway/ folders
- Testing: look for test files, test config
- Security: look for auth middleware, input validation, env vars usage
- API Design: look for route/controller/handler files, REST endpoints, OpenAPI specs
- Observability: look for logging imports, health check endpoints, middleware patterns
- Validation Loop: look for build/test/lint commands, CI config, zero-warning policy
- Banned Patterns: look for explicit anti-pattern rules, drift prevention directives

Report which atoms are:
- COVERED: rule exists and matches current atom
- OUTDATED: rule exists but atom has been updated (content differs)
- MISSING: project uses this pattern but has no rule for it
- EXTRA: project has rules not in any atom (could be project-specific or bloat)

### Step 4: Bloat Detection

Flag rules that Claude likely follows without being told:
- Generic best practices ("write clean code", "follow conventions")
- Language basics Claude already knows ("use const in JS")
- Duplicate rules (same intent expressed twice)
- Tutorial content (explaining what DDD *is* rather than how to enforce it)

For each flagged rule, explain why it's likely unnecessary and estimate tokens saved by removing it.

### Step 5: Infrastructure Check

Report what's missing from the Claude Code setup:

| Component | Status | Action |
|-----------|--------|--------|
| CLAUDE.md | Present/Missing | — |
| .claude/settings.json | Present/Missing | Run /bootstrap (hooks) |
| .claude/agents/ | Present/Missing | Run /bootstrap (subagents) |
| .claude/commands/ | Present/Missing | Run /bootstrap (commands) |
| INITIAL.md | Present/Missing | Run /bootstrap (PRP template) |
| PRPs/ | Present/Missing | Run /bootstrap (PRP output dir) |
| examples/ | Present/Missing | Run /bootstrap (code examples) |
| docs/project_notes/ | Present/Missing | Run /bootstrap (project memory) |
| CLAUDE.local.md | Present/Missing | Create for personal prefs |
| backbone.yml | Present/Missing | Generate with /bootstrap |
| .claude/skills/ | Present/Missing | Create skills directory |
| .claude/rules/ | Present/Missing | Create path-scoped rules |
| Context Hygiene section | Present/Missing | Add to CLAUDE.md |
| Verification section | Present/Missing | Add to CLAUDE.md |
| Model Selection section | Present/Missing | Add Haiku/Sonnet/Opus guidance |
| i18n section | Present/Missing (if applicable) | Add locales to project-specific section |
| mcp.json | Present/Missing | Configure MCP servers |

### Step 5b: Maturity Level Assessment

Assess the project's configuration maturity using the L0-L6 framework:

| Level | Name | Criteria |
|-------|------|----------|
| L0 | Absent | No CLAUDE.md exists |
| L1 | Basic | CLAUDE.md exists and is tracked in git |
| L2 | Scoped | Has explicit MUST/MUST NOT constraints (base atom rules present) |
| L3 | Structured | Uses external references or multiple files (@imports, .claude/rules/) |
| L4 | Abstracted | Path-scoped rules that load conditionally (frontmatter with `paths:`) |
| L5 | Maintained | L4 + backbone.yml exists + evidence of active maintenance (recent updates) |
| L6 | Adaptive | L5 + .claude/skills/ with custom skills + MCP integrations |

**Assessment logic:**
1. L0: No CLAUDE.md → stop here
2. L1: CLAUDE.md exists → check for constraint language
3. L2: Has MUST/MUST NOT/always/never rules → check for external references
4. L3: Has .claude/rules/ or @import references → check for path-scoping
5. L4: Rules have `paths:` frontmatter → check for backbone.yml + freshness
6. L5: backbone.yml exists AND CLAUDE.md updated within 30 days → check for skills + MCP
7. L6: .claude/skills/ has custom skills AND MCP config exists

**Report format:**
```
MATURITY LEVEL: L{N} ({Name})
  ✅ What qualifies: {evidence}
  ➡️  Next level: L{N+1} ({Name})
  📋 To get there: {specific action}
```

### Step 5c: Atom Conflict Detection

Cross-reference active atoms for:

1. **Duplicate rules** — Same intent expressed in multiple atoms
   - Scan base + active principles for overlapping rules
   - Report: `DUPLICATE: "{rule}" in {atom A} + {atom B} (~{tokens} tokens wasted)`

2. **Semantic conflicts** — Rules that contradict when combined
   - Report: `CONFLICT: {atom A} says "{rule}" but {atom B} says "{contradicting rule}"`

3. **Redundant project rules** — Project-specific section repeats an active atom's rule
   - Report: `REDUNDANT: project-specific "{rule}" already covered by {atom} atom`

### Step 5d: Hooks Analysis

If `.claude/settings.json` exists, analyze it:
- Which hook events are configured? (PostToolUse, Stop, SessionStart, etc.)
- Are format commands correct for the detected stack?
- Are test commands present in Stop hooks?
- Is SessionStart injecting useful context?
- Any missing hook events that would benefit this project?

If `.claude/settings.json` is missing, recommend the appropriate hooks template based on detected stack.

### Step 5e: Subagent Analysis

If `.claude/agents/` exists, analyze the agent definitions:
- Are planner, reviewer, and test-writer present?
- Are allowed_tools appropriate for the stack?
- Are agent roles clear and non-overlapping?

If agents are missing but project complexity warrants them (infer from file count, folder depth, dependency count), recommend adding them.

### Step 6: Generate Report

Present findings as a structured report:

```
PROJECT HEALTH REPORT
═══════════════════════

Maturity Level:   L{N} ({Name}) → next: L{N+1} ({action needed})
Token Budget:     {count}/2,500 ({status})
Atom Coverage:    {covered}/{total} atoms matched
Atom Conflicts:   {count} duplicates, {count} conflicts, {count} redundant
Bloat Detected:   {count} rules ({tokens} tokens recoverable)
Missing Setup:    {list}

COVERED ATOMS:
  ✅ base rules (with context hygiene)
  ✅ flutter stack
  ✅ solid principles
  ✅ security
  ✅ validation loop
  ✅ banned patterns

OUTDATED ATOMS (atom updated since project was bootstrapped):
  ⚠️  testing — atom added "mock at boundaries only" rule
  ⚠️  base — atom added context hygiene section

MISSING ATOMS (project uses pattern but no rule):
  ❌ clean-architecture — project has layered folders but no layer rules
  ❌ observability — project has logging but no structured logging rules

BLOAT CANDIDATES:
  🗑  "Use descriptive variable names" — Claude does this by default (~15 tokens)
  🗑  "Follow Flutter naming conventions" — baked into the model (~12 tokens)

CLAUDE CODE INFRASTRUCTURE:
  ✅ .claude/settings.json — hooks configured (PostToolUse, Stop, SessionStart)
  ✅ .claude/agents/ — 3 agents (planner, reviewer, test-writer)
  ✅ .claude/commands/ — {n} commands
  ❌ INITIAL.md — missing (no PRP workflow)
  ❌ PRPs/ — missing (no PRP output directory)
  ❌ examples/ — missing (no code example patterns)

RECOMMENDED ACTIONS:
  1. Remove {n} bloat rules (saves ~{tokens} tokens)
  2. Add missing atoms: {list}
  3. Update outdated atoms: {list}
  4. Set up PRP workflow: create INITIAL.md and PRPs/
  5. Create examples/ for non-obvious patterns
```

### Step 7: Offer Fixes

After presenting the report, ask the user:
- "Want me to apply these fixes now?" — directly edit CLAUDE.md + scaffold missing infrastructure
- "Want me to run /migrate instead?" — full restructure to composable format
- "Want me to just fix the bloat?" — remove unnecessary rules only
