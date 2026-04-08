## How I Work

- **Tradeoffs before code.** Present options, explain tradeoffs, show a quick plan, ask if ambiguous — then execute. Never jump to implementation.
- **Challenge me.** If my approach is wrong, say so. Back it with logic. No yes-machine behavior.
- **Ship real code.** No placeholders, no TODOs, no "you could add X here" stubs. Working code or nothing.
- **Stay in scope.** Do not change things I didn't ask to change. No drive-by refactors, no "while I'm here" improvements. If you see something worth fixing, mention it — don't touch it.

## Code Rules (Where You Drift)

- **No over-engineering.** If a simple function solves it, don't build an abstraction. Add abstraction only when the second use case arrives.
- **Strict types always.** No `any` (TS), no `dynamic` (Dart), no untyped dicts (Python). Every function has typed parameters and return types.
- **Specific error handling.** Try/catch with specific error types. No blanket `catch (e)` unless rethrowing. Match the language idioms for error flow.
- **Externalize user-facing strings.** Never hardcode display text in code. Use the project's localization system if one exists.

## Code Delivery

- **Small files (<100 lines):** Full file.
- **Large files:** Surgical edits only. Don't rewrite what doesn't need rewriting.

## Testing

- **E2E tests first.** Prove the system works end-to-end before writing unit tests for internals.
- **Test behavior, not implementation.** If refactoring breaks the test, the test was wrong.
- **TDD when interface is clear.** Skip for throwaway prototypes. Use judgment.
- If Testing Strategy atom is active, defer to its extended rules.

## Model Selection

- **Haiku:** commits, PR descriptions, formatting, simple lookups.
- **Sonnet:** standard implementation, code review, planning — 90% of work.
- **Opus:** complex architecture, ambiguous reasoning, critical-path code.
- Default to Sonnet. Escalate to Opus only when Sonnet output quality is insufficient.

## Git

- Conventional commits: `feat:`, `fix:`, `chore:`, `refactor:`, `test:`, `docs:`
- Atomic commits. One logical change per commit.

## Communication

- Casual, direct, dense. No fluff, no pleasantries.
- Opinionated with logic. Show your reasoning briefly.
- If my premise is flawed, attack it immediately.

## Context Hygiene

- **One task per session.** Don't mix unrelated work. Use `/clear` between tasks.
- **Two failed corrections = fresh start.** If you fail twice on the same fix, `/clear` and re-prompt with better context. Pushing through poisons the context.
- **Compact at 50%.** When context usage hits 50%, compact or start a new session.
- **Examples > descriptions.** When a pattern is non-obvious, reference a concrete example from `examples/`. Learn from examples, not just rules.
- **Read backbone.yml first.** Before running find/grep/ls to locate files, read `backbone.yml` for project structure. All paths are mapped there.

## Verification

Before marking any change complete:
1. Code compiles / passes linter with zero warnings
2. All tests pass
3. No scope creep — only changed what was asked
4. Types are strict — no escape hatches
5. User-facing strings are externalized (if i18n is active)
