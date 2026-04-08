## Testing Strategy

- **E2E tests first.** Prove the system works end-to-end before writing unit tests for internals.
- **Test behavior, not implementation.** If refactoring breaks the test, the test was wrong.
- **TDD when interface is clear.** Skip for throwaway prototypes. Use judgment.
- **Mock at boundaries only.** Mock external services, databases, APIs. Never mock internal classes.
- **Every bug gets a regression test.** Fix the bug, write the test that would have caught it, then commit both.
