## Banned Patterns (AI Drift Prevention)

These patterns are explicitly forbidden. They are the most common ways Claude drifts from intent.

- **No placeholder code.** No `// TODO: implement`, no `pass`, no stub returns. Every function must be complete or not written at all.
- **No catch-all error handling.** No bare `catch (e)`, `except Exception`, or `catch (_)` without rethrowing. Handle specific errors or let them propagate.
- **No premature abstraction.** No interfaces/abstract classes with a single implementation. No factory patterns for one product. Add abstraction when the second use case arrives.
- **No scope creep.** Do not modify files outside the current task scope. If something needs fixing elsewhere, flag it — don't touch it.
- **No "while I'm here" refactors.** Drive-by improvements compound into unreviewed changes. One task, one set of changes.
- **No tutorial comments.** Don't explain what the language does. Comments are only for non-obvious WHY.
- **No ignoring CLAUDE.md.** These rules exist because of past mistakes. Every rule was earned. Follow them.
