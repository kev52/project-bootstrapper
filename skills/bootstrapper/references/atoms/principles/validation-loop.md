## Validation Loop

- **Run after every change.** Build, lint, typecheck, test — every time. No exceptions. If it's too slow, fix the pipeline, don't skip validation.
- **Fail fast.** Wire linter + typecheck + tests into a single command. If any step fails, stop and fix before moving on.
- **CI must match local.** Whatever runs locally must run in CI. No "works on my machine."
- **Zero-warning policy.** Warnings become errors. Suppress nothing without a documented reason.
- **Self-correction loop.** When Claude introduces a failure: read the error, fix it, re-run validation. Don't ask the user unless stuck after 2 attempts.
