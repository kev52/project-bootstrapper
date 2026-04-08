# Changelog

All notable changes to the Project Bootstrapper plugin.

## [2.0.0] - 2026-04-06

### Added
- **backbone.yml generation** — Project topology map templates for TypeScript, Python, Flutter, and generic stacks. Eliminates exploration tax by giving Claude a map before it starts grepping.
- **Context Engineering atom** — 9 rules for strategic information architecture: front-load constraints, numbered requirements, session decay management, compress context, examples as specs.
- **Agentic Delegation atom** — 9 rules from research on effective agent workflows: plan before execution, git before every task, write-the-test-first, decompose to verification boundary, review diffs not files.
- **Model Selection in base atom** — Haiku/Sonnet/Opus tiering with routing guidance. Default Sonnet, escalate to Opus only when output quality is insufficient.
- **L0-L6 maturity scoring in auditor** — Scores projects against a 7-level maturity framework from Absent (L0) to Adaptive (L6).
- **Atom conflict detection in auditor** — Detects duplicate rules, semantic conflicts, and redundant project-specific rules across atoms.
- **Skills directory scaffolding in bootstrapper** — Creates `.claude/skills/README.md` and `_template/SKILL.md` during bootstrap.
- **Orchestrator agent template** — Hierarchical coordinator for complex projects. Decomposes tasks, delegates to planner/reviewer/test-writer, handles 2-failure escalation.
- **OpenWolf compatibility note** — Bootstrap report mentions OpenWolf middleware for token reduction.

### Changed
- **Reviewer promotion workflow** — Findings that recur 3+ times across reviews auto-surface as promotion suggestions with ready-to-paste CLAUDE.md rules.
- **Bootstrapper interview** — Added Context Engineering and Agentic Delegation atom recommendations. Agent team question now offers Flat/Hierarchical/No options.
- **Auditor infrastructure check** — Now checks for backbone.yml, .claude/skills/, .claude/rules/, Model Selection section, mcp.json.

### Removed
- **"No over-commenting" rule** from base atom — Claude does this by default.
- **"Readability > Performance" rule** from base atom — Claude does this by default.
- **"Offer a distinct alternative" in Code Delivery** — Claude does this by default.
- **Testing overlap** in base atom — Now defers to Testing atom when active, avoiding duplicate rules.

## [1.0.0] - 2026-03-24

### Added
- Initial release with bootstrapper, code reviewer, auditor, project memory skills
- 11 slash commands (bootstrap, review, audit, migrate, sync, generate-prp, execute-prp, security-scan, setup-ci, gen-changelog, commit, pr)
- Composable atom system: base + stack + principles
- Stacks: TypeScript, Python, Flutter
- Principles: SOLID, DDD, Clean Architecture, SOA, Testing, Security, API Design, Observability, Validation Loop, Banned Patterns
- Hooks templates per stack (format, test, session context)
- Subagent templates: planner, reviewer, test-writer
- PRP workflow with INITIAL.md and prp-base templates
- Project memory system (bugs, decisions, key facts)
