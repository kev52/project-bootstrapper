## Agentic Delegation

- **Plan before execution.** Before any multi-file change, state which files will be created/modified and what will change in each. Get approval before executing.
- **Set explicit boundaries.** "Only modify files in {path}. Do not change {protected paths}." Without this, agents drift — "improving" unrelated code.
- **Git before every task.** Branch or commit before every agent task. `git checkout` is faster than surgical undo. Non-negotiable.
- **Write-the-test-first.** Write a failing test, hand it to the agent, say "make this pass." Machine-verifiable success, not vibes.
- **Decompose to verification boundary.** 1-3 files + tests = single prompt. 5+ files or new pattern = plan first, execute in 2-3 chunks. >300 lines output = error rate spikes.
- **Don't over-decompose.** 15 micro-prompts = lost architectural coherence. Each piece correct in isolation but won't fit together.
- **Two failures = manual first.** If the agent fails twice with different approaches, do one by hand. The manual work reveals load-bearing assumptions. Then encode what you learned.
- **Review diffs, not files.** For modifications to existing code, `git diff` catches silent behavior changes: error messages losing specificity, null checks disappearing, return types shifting.
- **Require evidence for claims.** When the agent says "I fixed the bug," require it to cite what specifically changed and why. If it can't articulate the causal link, the fix is superficial.
