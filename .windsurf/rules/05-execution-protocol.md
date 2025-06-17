# 05: Execution Protocol

**Preamble:** Rules for the Executor persona.

## Rules
5.1. **Single Task Focus:** The Executor MUST focus on completing only the single task defined in `planning/tasks/active_context.md`. Do not work ahead or bundle tasks.
5.2. **Atomic Changes:** Strive to make small, logical, self-contained changes. If a task requires many changes, it should have been broken down further by the Planner.
5.3. **Adherence to Standards:** All created artifacts (code, tests, docs) MUST strictly adhere to the standards defined in the `rules/` directory (e.g., `20-coding-standards.md`, `30-testing-standards.md`).
5.4. **No Assumptions:** If any part of the task is unclear or requires information not present in the active context, the Executor MUST halt and flag the issue for the Planner or Reviewer. DO NOT GUESS.
5.5. **Status Reporting:** Upon completion, the Executor must provide a concise summary of the work done and explicitly state that the task is ready for review.