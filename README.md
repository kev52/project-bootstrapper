# Project Bootstrapper

Full lifecycle management for Claude Code project configuration. Composable CLAUDE.md from atomic rules, backbone.yml for project topology, agent teams, hooks, skills scaffolding, and PRP workflows.

## Installation

### Via Marketplace (recommended)

```bash
claude plugin marketplace add your-username/project-bootstrapper
claude plugin install project-bootstrapper
```

Replace `your-username` with the GitHub org or username hosting this repo.

### Via Cowork

Upload the `.plugin` ZIP through your organization's Cowork admin panel under Plugins.

### Manual

Clone this repo into your local plugins directory or reference it directly:

```bash
git clone https://github.com/your-username/project-bootstrapper.git ~/.claude/plugins/project-bootstrapper
```

## What It Does

Bootstrap a new project with one command and get a composable CLAUDE.md, git hooks, scoped subagents, slash commands, and a structured feature workflow. Then maintain it with audits, reviews, and syncs.

### Core Loop

```
/bootstrap  ->  develop + /review  ->  /audit (monthly)  ->  /sync
```

## Commands

| Command | Purpose |
|---------|---------|
| `/bootstrap` | Generate full config from interactive interview |
| `/review` | Review code against active CLAUDE.md atoms |
| `/audit` | Health report + L0-L6 maturity score + atom conflict detection |
| `/migrate` | Convert monolithic CLAUDE.md to composable atom system |
| `/sync` | Update atom sections with latest versions |
| `/generate-prp` | Generate a Product Requirements Prompt from a feature brief |
| `/execute-prp` | Execute a PRP task by task with validation |
| `/security-scan` | Scan for vulnerabilities and hardcoded secrets |
| `/setup-ci` | Scaffold GitHub Actions CI workflow |
| `/gen-changelog` | Generate CHANGELOG from conventional commits |
| `/commit` | Conventional commit from staged changes (Haiku) |
| `/pr` | PR description from branch diff (Haiku) |

## Skills

| Skill | Triggers |
|-------|----------|
| Bootstrapper | "bootstrap a project", "generate CLAUDE.md", "set up Claude Code" |
| Code Reviewer | "review this code", "check this PR", "does this follow our patterns" |
| Auditor | "audit my CLAUDE.md", "check my setup", "is my config bloated" |
| Project Memory | "log a bug", "record a decision", "what bugs have we seen" |

## Architecture

Configuration is composed from atomic pieces:

```
base (always)  +  stack (pick one)  +  principles (pick any)  =  CLAUDE.md
  ~500 tokens     ~280-350 tokens      ~100-350 each            <2,500 total
```

### Available Atoms

**Stacks:** Flutter, TypeScript, Python

**Principles:** SOLID, DDD, Clean Architecture, SOA, Testing, Security, API Design, Observability, Validation Loop, Banned Patterns, Context Engineering, Agentic Delegation

### What Gets Generated

| What | Location | Purpose |
|------|----------|---------|
| Rules | `CLAUDE.md` | Composable rules from atoms |
| Backbone | `backbone.yml` | Project topology map — eliminates exploration tax |
| Hooks | `.claude/settings.json` | Auto-format, auto-test, session context injection |
| Subagents | `.claude/agents/*.md` | Planner, reviewer, test-writer (+ orchestrator for hierarchical teams) |
| Commands | `.claude/commands/*.md` | Slash commands with YAML frontmatter |
| Skills Dir | `.claude/skills/` | Scaffolded skills directory with template |
| PRP Workflow | `INITIAL.md` + `PRPs/` | Feature brief -> plan -> execution |
| Project Memory | `docs/project_notes/` | Bugs, decisions, key facts across sessions |

### Agent Teams

Two modes depending on project complexity:

**Flat team** (default): Planner + Reviewer + Test Writer — each with scoped permissions.

**Hierarchical team**: Adds an Orchestrator agent that decomposes tasks, delegates to specialists, aggregates results, and handles escalation. Recommended for projects with 5+ services or complex cross-cutting concerns.

### Maturity Levels (L0-L6)

The auditor scores your project against a maturity framework:

| Level | Name | Description |
|-------|------|-------------|
| L0 | Absent | No CLAUDE.md |
| L1 | Basic | Single CLAUDE.md, prose rules |
| L2 | Scoped | MUST/MUST NOT rules, clear boundaries |
| L3 | Structured | External refs, examples, backbone.yml |
| L4 | Abstracted | Path-scoped rules, composable atoms |
| L5 | Maintained | Audit cycle, backbone.yml as source of truth |
| L6 | Adaptive | Skills, MCP integrations, agent teams |

### Model Selection

The base atom includes smart model routing:

- **Haiku**: commits, PR descriptions, formatting, simple lookups
- **Sonnet**: standard implementation, code review, planning (90% of work)
- **Opus**: complex architecture, ambiguous reasoning, critical-path code

## Backbone Templates

Backbone.yml is a project topology map that eliminates the "exploration tax" — tokens and time wasted on Claude guessing your project structure. Templates included for TypeScript, Python, Flutter, and a generic fallback.

## Customization

- **Atoms**: `skills/bootstrapper/references/atoms/` — edit base, stack, or principle atoms
- **Hooks**: `skills/bootstrapper/references/hooks/` — customize per-stack automation
- **Agents**: `skills/bootstrapper/references/agents/` — adjust roles and permissions
- **Backbone**: `skills/bootstrapper/references/backbone/` — add or modify stack templates

## Maintenance

1. **Monthly**: Run `/audit` on active projects — check maturity level and atom conflicts
2. **When Claude drifts**: Add the corrective rule to the right atom
3. **When atoms update**: Run `/sync` on active projects
4. **Quarterly**: Prune rules Claude follows without being told
5. **Per feature**: Fill INITIAL.md -> `/generate-prp` -> review -> `/execute-prp`
6. **Code reviews**: Findings that recur 3+ times get auto-promoted to CLAUDE.md rules

## Versioning

This plugin follows semantic versioning. Check the [CHANGELOG](CHANGELOG.md) for release notes.

## License

MIT
