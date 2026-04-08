---
name: code-reviewer
description: >
  This skill should be used when the user says "review this code", "code review",
  "check this PR", "review my changes", "does this follow our patterns",
  "check for violations", or wants code reviewed against the project's
  active CLAUDE.md atoms and architecture principles.
version: 1.0.0
---

# Code Reviewer

Review code against the active atoms in the project's CLAUDE.md. Not a generic linter — this catches drift from the project's declared architecture, principles, and conventions.

## Input

The user provides one of:
- **Files/folders** to review (e.g., "review lib/services/")
- **Git diff** (e.g., "review my staged changes", "review this PR")
- **Nothing** — default to staged changes (`git diff --cached`), or if nothing staged, last commit (`git diff HEAD~1`)

## Process

### Step 1: Load Active Rules

Read the project's CLAUDE.md. Identify which atoms are active by matching section headers and `---` separators:

- Base rules → always active
- Stack rules → detect from Commands section or framework imports
- Principle atoms → match headers (SOLID, DDD, Clean Architecture, SOA, Testing Strategy, Security, API Design, Observability, Validation Loop, Banned Patterns)

Also load the Project-Specific section at the bottom for custom rules.

If no CLAUDE.md exists, warn and review against only the base atom from `${CLAUDE_PLUGIN_ROOT}/skills/bootstrapper/references/atoms/base/CLAUDE.md`.

### Step 2: Gather Code to Review

Based on user input:

**Files/folders:**
- Read each file. Skip generated files (`*.g.dart`, `*.generated.*`, `node_modules/`, `build/`, `dist/`).
- For large files (>300 lines), focus on public API, class structure, and recent changes.

**Git diff:**
- Run `git diff --cached` (staged) or `git diff HEAD~1` (last commit) or the specified range.
- Parse the diff to understand what changed and in which files.
- Read the full files for context around the changed lines.

**PR review:**
- Run `git diff main...HEAD` (or the appropriate base branch).
- Focus on all commits in the branch.

### Step 3: Run Review Checks

For each file/change, check against every active atom. Organize findings by severity.

#### Base Rules Checks
- [ ] **Over-engineering:** Abstraction without a second use case? Interface with only one implementation and no clear extensibility need?
- [ ] **Strict types:** Any `any` (TS), `dynamic` (Dart), untyped dicts (Python), missing return types?
- [ ] **Error handling:** Blanket `catch (e)` without specific types? Swallowed errors? Missing rethrow?
- [ ] **String externalization:** Hardcoded user-facing strings? (If project has an i18n section in CLAUDE.md, also check locale coverage.)
- [ ] **Scope creep:** Changes outside the stated scope?
- [ ] **TODOs/placeholders:** Stubs, "implement later" comments, placeholder returns?
- [ ] **Comments:** Comments explaining WHAT instead of WHY?

#### Banned Patterns Checks (if active)
- [ ] **Placeholder code:** Any `// TODO`, `pass`, `NotImplementedError`, or stub functions?
- [ ] **Catch-all errors:** Bare `catch (e)`, `except Exception`, `catch (_)` without rethrow?
- [ ] **Premature abstraction:** Interface/abstract class with single implementation? Factory for one product?
- [ ] **Scope creep:** Files modified outside the current task's stated scope?
- [ ] **Drive-by refactors:** Unrelated improvements bundled into the current change?
- [ ] **Tutorial comments:** Comments explaining language features rather than business logic?
- [ ] **CLAUDE.md violations:** Any code that directly contradicts a rule in the project's CLAUDE.md?

#### Validation Loop Checks (if active)
- [ ] **Missing tests:** New feature code without corresponding tests?
- [ ] **Linter compliance:** Would the code pass the project's linter with zero warnings?
- [ ] **Type checking:** Would `tsc --noEmit` / `flutter analyze` / `ruff check` pass?
- [ ] **CI alignment:** Do local validation commands match what CI runs?

#### SOLID Checks (if active)
- [ ] **SRP:** Class with multiple reasons to change? (e.g., validation + persistence in one class)
- [ ] **OCP:** Growing if/else or switch chains instead of new implementations?
- [ ] **LSP:** Overridden methods that break the parent contract? NotImplemented exceptions?
- [ ] **ISP:** Fat interfaces where implementors throw on unused methods?
- [ ] **DIP:** Concrete dependencies instead of interfaces at service boundaries? Static access? Service locators?

#### DDD Checks (if active)
- [ ] **Anemic models:** Entity classes with only getters/setters, no behavior?
- [ ] **Raw primitives:** Email, Money, PhoneNumber as raw strings/numbers instead of value objects?
- [ ] **Invariant leakage:** Validation in use cases or controllers instead of aggregates?
- [ ] **Direct coupling:** Aggregate A calling Aggregate B directly instead of via events?
- [ ] **Naming:** Generic names (DataManager, ServiceHelper, BaseProcessor) instead of ubiquitous language?
- [ ] **Domain purity:** Domain layer importing framework, ORM, or HTTP code?

#### Clean Architecture Checks (if active)
- [ ] **Layer violations:** Domain importing from infrastructure or presentation? Presentation touching domain directly?
- [ ] **Business logic placement:** Business rules in controllers, handlers, or infrastructure?
- [ ] **Use case structure:** Use cases with multiple public methods? Use cases containing business rules?
- [ ] **Feature folder structure:** Code organized by technical layer instead of by feature?
- [ ] **Missing interfaces:** Services without an interface?

#### SOA Checks (if active)
- [ ] **Shared state:** Services accessing another service's database or internal state?
- [ ] **Domain leakage:** Domain entities crossing context boundaries instead of DTOs/events?
- [ ] **Missing anti-corruption layers:** External API responses passed through without mapping?
- [ ] **No failure handling:** Missing circuit breakers, retries, or graceful degradation?

#### Testing Checks (if active)
- [ ] **Implementation coupling:** Tests that break on refactoring? Mocking internal classes?
- [ ] **Missing regression test:** Bug fix without an accompanying test?
- [ ] **Boundary mocking:** Mocking internal collaborators instead of external boundaries?
- [ ] **Test-to-code ratio:** New feature code without any tests?

#### Security Checks (if active)
- [ ] **Untrusted input:** User input used without validation or sanitization?
- [ ] **SQL/query injection:** String interpolation or concatenation in database queries?
- [ ] **Hardcoded secrets:** API keys, tokens, passwords, or connection strings in source code?
- [ ] **Missing auth:** Endpoints/routes without explicit authentication declarations?
- [ ] **Unescaped output:** User input rendered in HTML/templates without encoding?
- [ ] **Fail-open:** Auth/permission checks that fall through to "allow" on error?
- [ ] **Overprivileged:** Wildcard CORS, admin-level DB users, or broad IAM permissions?

#### API Design Checks (if active)
- [ ] **Verb-noun mismatch:** Routes like `GET /getUser` instead of `GET /users/:id`?
- [ ] **Inconsistent errors:** Error responses with different shapes across endpoints?
- [ ] **Wrong status codes:** `200` with `{ success: false }` or wrong HTTP codes?
- [ ] **Missing pagination:** List endpoints returning unbounded arrays?
- [ ] **No input validation:** Request bodies accepted without schema validation?
- [ ] **Non-idempotent mutations:** POST/PUT that can't safely be retried?

#### Observability Checks (if active)
- [ ] **Unstructured logging:** `print()`, `console.log()` with plain strings instead of structured format?
- [ ] **Wrong log levels:** `ERROR` for non-errors, or `INFO` for debug-only detail?
- [ ] **Missing correlation IDs:** Request handling without propagating trace/correlation IDs?
- [ ] **PII in logs:** Email, password, token, or other sensitive data in log output?
- [ ] **Missing error context:** Error logs without describing what failed and what input caused it?
- [ ] **No health check:** Deployed service without a `/health` endpoint?

#### Stack-Specific Checks
**Flutter (if active):**
- [ ] Material widgets instead of Cupertino (if CupertinoApp)?
- [ ] Missing `const` constructors?
- [ ] `mounted` check after async gap in StatefulWidget?
- [ ] Bare `flutter`/`dart` commands instead of `fvm`?
- [ ] Missing locale files for i18n changes? (Only if i18n section exists in CLAUDE.md — check all declared locales.)

**TypeScript (if active):**
- [ ] Missing Zod validation at API boundaries?
- [ ] `any` types or implicit any?
- [ ] Default exports instead of named exports?
- [ ] Callback hell instead of async/await?

**Python (if active):**
- [ ] Missing type hints?
- [ ] Dict/list where Pydantic models should be used?
- [ ] `os.path` instead of `pathlib`?
- [ ] Generic `Exception` instead of custom error classes?

### Step 4: Generate Report

Present findings grouped by severity, then by file.

```
CODE REVIEW REPORT
═══════════════════

Scope: {files reviewed} files, {lines} lines changed
Active atoms: base, {stack}, {principles...}

🔴 CRITICAL ({count})
  Issues that violate core architecture rules and must be fixed.

  {file}:{line} — {issue}
    Rule: {atom} → {specific rule}
    Fix: {concrete suggestion}

🟡 WARNING ({count})
  Issues that indicate drift from declared patterns.

  {file}:{line} — {issue}
    Rule: {atom} → {specific rule}
    Fix: {concrete suggestion}

💡 SUGGESTION ({count})
  Not violations, but opportunities to improve.

  {file}:{line} — {suggestion}

✅ GOOD PATTERNS SPOTTED
  Things done well worth highlighting.

  {file} — {what's good about it}

SUMMARY
  Critical: {n} (must fix)
  Warnings: {n} (should fix)
  Suggestions: {n} (consider)
  Clean files: {n}/{total}
```

### Step 5: Severity Classification

**🔴 CRITICAL** — Direct rule violations that will compound into technical debt:
- Layer violations (Clean Architecture)
- Domain purity breaks (DDD)
- Type escape hatches (`any`, `dynamic`)
- Missing error handling
- Shared state between services (SOA)
- SQL injection / untrusted input (Security)
- Hardcoded secrets (Security)
- PII in logs (Observability)
- Fail-open auth (Security)
- Placeholder code (Banned Patterns)
- CLAUDE.md rule violations (Banned Patterns)

**🟡 WARNING** — Pattern drift that weakens architecture over time:
- Anemic models when DDD is active
- Missing interfaces at service boundaries
- Over-engineering / premature abstraction
- Growing switch/if chains (OCP violation)
- Implementation-coupled tests
- Missing auth declarations on endpoints (Security)
- Unstructured logging (Observability)
- Inconsistent error response shapes (API Design)
- Missing pagination on list endpoints (API Design)
- Missing tests for new features (Validation Loop)
- Drive-by refactors (Banned Patterns)

**💡 SUGGESTION** — Not wrong, but could be better:
- Comment quality
- Naming improvements
- Test coverage gaps
- Code organization
- Missing correlation IDs (Observability)
- Idempotency opportunities (API Design)
- Health check endpoints (Observability)
- CI/local command alignment (Validation Loop)

### Step 6: Offer Actions

After the report, offer:
- **"Fix critical issues"** — Apply fixes for all critical findings
- **"Fix all"** — Apply fixes for critical + warning findings
- **"Log to project memory"** — Record recurring patterns in `docs/project_notes/bugs.md`
- **"Update CLAUDE.md"** — If a new pattern emerged that should be a rule, suggest adding it to the project-specific section

### Promotion Workflow

Track recurring findings to surface patterns worth codifying:

**After each review:**
1. Check if `docs/project_notes/review_patterns.md` exists. If not, create it with header: `# Recurring Review Patterns`
2. For each finding (critical or warning), check if the same rule violation already has an entry
3. If exists: increment the count and add the new file to the list
4. If new: add an entry:
```markdown
### {Rule violation description}
- **Atom:** {which atom this relates to}
- **Count:** 1
- **Files:** {file1}
- **First seen:** {date}
```

**When count reaches 3:**
Surface a promotion suggestion in the review report:
```
🔄 RECURRING PATTERN DETECTED
  "{rule violation}" has appeared in 3+ reviews ({count} total, {files})

  Suggested CLAUDE.md rule:
    - **{Concise rule statement}** — {why, based on the pattern}

  Add to: {project-specific section / suggest as atom update}
```

**Promotion decision tree:**
- Pattern is project-specific (naming convention, internal API shape) → add to project-specific section of CLAUDE.md
- Pattern is stack-general (TypeScript anti-pattern, Python idiom) → suggest as stack atom update
- Pattern is universal (applies to any project) → suggest as base or principle atom update
