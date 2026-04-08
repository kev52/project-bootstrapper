---
name: bootstrapper
description: >
  This skill should be used when the user says "bootstrap a project",
  "set up a new project", "generate CLAUDE.md", "generate AGENTS.md",
  "init project config", "start a new app", or needs to generate
  AI configuration files for a new or existing codebase.
version: 1.0.0
---

# Project Bootstrapper

Generate a complete Claude Code configuration for any project: CLAUDE.md, `.claude/settings.json` (hooks), `.claude/agents/` (subagents), `.claude/commands/`, PRP infrastructure, and project memory.

## Two Entry Modes

### Mode 1: Interview (Default)

Ask these questions using AskUserQuestion. Recommend based on complexity.

**Q1: Stack**
- Flutter/Dart
- TypeScript/Node
- Python
- None / Other

**Q2: Project complexity**
- Script / CLI / prototype (lightweight)
- CRUD API / thin service (moderate)
- App with real business logic (complex)
- Distributed system / microservices (enterprise)

**Q3: Architecture principles** (multi-select, recommend based on Q2)
- SOLID (recommend for moderate+)
- DDD (recommend for complex+)
- Clean Architecture (recommend for complex+)
- SOA (recommend for enterprise only)
- Testing strategy (recommend for moderate+)
- Security (recommend for moderate+, especially with user auth or external APIs)
- API Design (recommend for any project exposing or consuming APIs)
- Observability (recommend for complex+, any deployed service)
- Validation Loop (recommend for ALL projects — this is the #1 productivity multiplier)
- Banned Patterns (recommend for ALL projects — prevents AI drift)
- Context Engineering (recommend for ALL projects — the #1 differentiator for AI-assisted workflows)
- Agentic Delegation (recommend for complex+ — rules for delegating multi-file tasks to agents)

**Q4: Internationalization**
- No i18n needed (single language)
- Yes — specify base locale and supported languages

If user selects yes, ask for: base locale (e.g., `en`) and list of supported locales (e.g., `en, de, es, fr`). These go in the project-specific section of CLAUDE.md, not in any atom.

**Q5: Project name**

**Q6: Claude Code hooks** (multi-select)
- Auto-format on every file edit (Recommended — PostToolUse hook)
- Auto-run tests when Claude finishes (Recommended for moderate+ — Stop hook)
- Inject git status + TODOs on session start (Recommended — SessionStart hook)
- No hooks — I'll manage manually

**Q7: Subagents**
- Flat team — generate planner, reviewer, and test-writer agents (Recommended for complex)
- Hierarchical team — add orchestrator agent that coordinates specialists (Recommended for enterprise/distributed)
- No — I'll work with a single Claude session

### Mode 2: Spec Upload

When the user provides a spec/PRD instead of answering questions, analyze it to infer:
- **Stack**: from mentions of frameworks, languages, package managers
- **Complexity**: from entity count, integration points, business rule density
- **Architecture needs**: map complexity to principle recommendations
- **Testing needs**: from quality/compliance requirements
- **i18n needs**: from mentions of locales, translations, multi-language support
- **Security needs**: from auth mentions, user data handling, payment processing
- **API design needs**: from API endpoints, REST/GraphQL mentions, integrations
- **Observability needs**: from logging, monitoring, alerting, production deployment mentions
- **Hooks needs**: always recommend auto-format + auto-test for moderate+
- **Subagent needs**: recommend for complex+ projects

Explain each inference briefly before generating. Let the user override any inference.

## Generation Process

After gathering inputs (interview or spec), generate all files in the project root.

### Step 1: Compose CLAUDE.md

Read the atom files from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/atoms/` and compose:

1. Start with header: `# CLAUDE.md — {project_name}`
2. Add date stamp: `> Auto-composed on {date}. Audit monthly. Every line must earn its place.`
3. Append `base/CLAUDE.md` (always)
4. Append selected stack file from `stacks/` with `---` separator
5. Append each selected principle from `principles/` with `---` separators
6. If i18n was selected, add an `## i18n` section with base locale, supported locales, and tooling notes (e.g., Slang for Flutter, i18next for TS, gettext for Python)
7. Add project-specific placeholder section at bottom

Write to `CLAUDE.md` in project root.

**Token budget**: Total must be under 2,500 tokens. Report word count and estimated tokens after generation.

### Step 1b: Generate backbone.yml

Read the appropriate backbone template from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/backbone/{stack}.yml` (use `generic.yml` for "None / Other" stack).

1. Replace `{project_name}` with the actual project name
2. If the project has known directories (from spec upload or existing codebase), update the `structure` section to match reality
3. Write to `backbone.yml` in project root

The base atom already references backbone.yml in its Context Hygiene section, so no additional CLAUDE.md wiring is needed.

### Step 2: Generate Claude Code Hooks

If the user selected any hooks in Q6, read the appropriate hooks template from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/hooks/{stack}.json` and generate `.claude/settings.json`.

Only include the hook events the user selected:
- Auto-format → `PostToolUse` with Write|Edit matcher
- Auto-test → `Stop` with build + test commands
- Session context → `SessionStart` with git status + TODO grep

If the user selected "None / Other" stack, use generic commands:
- Format: skip (no default formatter)
- Test: `echo 'No test command configured — update .claude/settings.json'`
- Session: git status only

### Step 3: Generate Subagents

If the user selected subagents in Q7, read the agent templates from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/agents/` and generate:

**Flat team (always when subagents selected):**
- `.claude/agents/planner.md` — architect agent, read-only, produces implementation plans
- `.claude/agents/reviewer.md` — review agent, read-only, checks against CLAUDE.md
- `.claude/agents/test-writer.md` — test agent, can write test files only

**Hierarchical team (when "Hierarchical team" selected):**
- All flat team agents, plus:
- `.claude/agents/orchestrator.md` — coordinator agent that decomposes complex tasks, delegates to planner/reviewer/test-writer, aggregates results, and handles escalation

Adapt allowed_tools based on stack (e.g., Flutter agents get `Bash(fvm:*)`, TS agents get `Bash(npm:*)`, `Bash(npx:*)`).

### Step 4: Scaffold Slash Commands

Generate `.claude/commands/` with markdown files that include YAML frontmatter (`description`, `allowed-tools`).

Read command templates from `${CLAUDE_PLUGIN_ROOT}/commands/` and adapt them.

**Universal commands** (always include):
- `commit.md` — conventional commit generator
- `pr.md` — PR description generator

**Principle-based commands** (include based on selections):
- If Security atom is active: `security-scan.md`
- If any atom is active: `review.md`

**Stack-specific commands** (include based on stack selection):
- Flutter: include `analyze.md` (runs `fvm flutter analyze`)
- TypeScript: include `typecheck.md` (runs `npx tsc --noEmit`)
- Python: include `lint.md` (runs `ruff check .`)

### Step 5: Set Up PRP Infrastructure

Create the PRP workflow scaffolding:

1. Copy `INITIAL.md` template from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/prp/INITIAL.md.template` to project root, replacing `{project_name}` with the actual project name
2. Create `PRPs/` directory with a `.gitkeep` file
3. Ensure `generate-prp.md` and `execute-prp.md` commands exist in `.claude/commands/`

### Step 6: Create Examples Directory

Create `examples/` directory with a `README.md`:

```markdown
# Examples

Add concrete code examples here for patterns that CLAUDE.md describes.
Claude learns better from examples than from rules alone.

## Usage

Reference examples in CLAUDE.md like:
> See `examples/error-handling.ts` for the pattern.

## Structure

- One file per pattern
- Name files descriptively: `{pattern-name}.{ext}`
- Keep examples minimal — show the pattern, nothing else
```

### Step 6b: Scaffold Skills Directory

Create `.claude/skills/` with a template for the user's first skill:

1. Create `.claude/skills/README.md`:
```markdown
# Skills

Skills are installable expertise that Claude loads on demand. If you repeat a structured prompt more than twice, turn it into a Skill.

## Creating a Skill

1. Create a directory: `.claude/skills/{skill-name}/`
2. Add a `SKILL.md` file with frontmatter (name, description) and instructions
3. Claude auto-discovers skills by matching the description to user requests

## Template

Copy `_template/SKILL.md` and customize.
```

2. Create `.claude/skills/_template/SKILL.md`:
```markdown
---
name: skill-name
description: >
  Trigger phrases: "do X", "run Y", "create Z".
  Describe when this skill should activate.
---

# Skill Name

## What This Skill Does
[Describe the outcome this skill produces]

## Process
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Output Format
[Define what the output looks like]
```

### Step 7: Create Project Memory

Create `docs/project_notes/` with empty starter files:
- `bugs.md` — with header: `# Known Bugs & Solutions`
- `decisions.md` — with header: `# Architecture Decisions`
- `key_facts.md` — with header: `# Key Implementation Details`

### Step 8: Report

After generating everything, print a summary:

```
Files created:
  CLAUDE.md                              (~{tokens} tokens)
  backbone.yml                           (project structure map)
  INITIAL.md                             (feature brief template)

  .claude/settings.json                  (hooks: {list of active hooks})
  .claude/agents/planner.md              (if subagents selected)
  .claude/agents/reviewer.md             (if subagents selected)
  .claude/agents/test-writer.md          (if subagents selected)
  .claude/agents/orchestrator.md         (if hierarchical team selected)
  .claude/commands/commit.md
  .claude/commands/pr.md
  .claude/commands/generate-prp.md
  .claude/commands/execute-prp.md
  .claude/commands/{additional}.md       (based on selections)

  .claude/skills/README.md               (skills guide)
  .claude/skills/_template/SKILL.md      (skill template)
  PRPs/                                  (PRP output directory)
  examples/                              (code example patterns)
  docs/project_notes/bugs.md
  docs/project_notes/decisions.md
  docs/project_notes/key_facts.md

Maturity Level: L{level} ({name})
  L4 = path-scoped rules + backbone.yml
  L5 = L4 + active maintenance discipline
  L6 = L5 + skills + MCP integrations
  You are starting at L{level}. Run /audit monthly to track progress.

Token budget: {tokens}/2,500 ({percentage}% used)

Active hooks:
  PostToolUse: {format command}          (auto-format on edit)
  Stop: {test command}                   (auto-test on completion)
  SessionStart: git status + TODOs       (context injection)

Subagents: {flat team / hierarchical team / none}
  planner — plans before code (read-only)
  reviewer — reviews against CLAUDE.md (read-only)
  test-writer — writes tests from specs
  orchestrator — coordinates specialists (hierarchical only)

Next steps:
  1. Fill out INITIAL.md for your first feature
  2. Run /generate-prp to create an implementation plan
  3. Review the PRP, then run /execute-prp to build
  4. Add project-specific gotchas to the bottom of CLAUDE.md
  5. Update backbone.yml if your directory structure differs from the template
  6. Commit all generated files to git
  7. Create CLAUDE.local.md for personal preferences (.gitignore it)
  8. Create your first Skill when you notice a repeated prompt pattern
  9. Audit monthly — delete rules Claude follows without being told

Token optimization (optional):
  For large projects, consider OpenWolf (npm install -g openwolf) as a
  complementary middleware layer. It adds file indexing, session memory,
  and redundant-read prevention. Measured 65% avg token reduction across
  20 projects. Works alongside this plugin's atoms and hooks.
```

## MCP Server Recommendations

Based on stack selection, recommend from this list:

**All stacks:**
- project-memory (if not already using docs/project_notes/)

**TypeScript/Node:**
- Desktop Commander — filesystem + terminal with long-running commands
- Code Executor — sandboxed code execution

**Python:**
- Code Executor — sandboxed Python execution
- Bright Data Web MCP — if project involves web scraping

**With external APIs:**
- Graphiti MCP — persistent knowledge graph memory

**With GitHub:**
- GitIngest MCP — skim repos without cloning

## Additional Resources

Read the atom source files in `references/atoms/` for the content of each composable piece.
Read `references/hooks/` for the hooks configuration per stack.
Read `references/agents/` for the subagent templates.
Read `references/prp/` for the PRP templates.
