## Context Engineering

- **Front-load constraints.** Put requirements and constraints BEFORE code blocks in prompts. Top of context gets more attention.
- **Number requirements.** Agents track numbered lists more reliably than prose paragraphs.
- **Specify outcomes, not implementation.** State what to achieve, constraints, and verification criteria — not how to code it.
- **Examples as specs.** One input/output pair eliminates more ambiguity than a paragraph. Include at least one example per task.
- **Compress context.** 2,000 words of spec beats 50,000 lines of code. If you can't write a short spec, you don't understand the problem well enough to delegate.
- **Session decay is real.** After ~10 messages on the same implementation, restart with a clean prompt and accumulated learnings. Don't push through a polluted thread.
- **Summarize before continuing.** On multi-step tasks, restate what's completed and what's next before starting the next step.
- **Re-include by reference.** Never say "the function from earlier." Reference the file path or re-paste the code.
- **Read the map first.** Before exploring with find/grep/ls, check `backbone.yml` for known project structure.
