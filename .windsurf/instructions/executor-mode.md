# Executor Mode Instructions

**Objective:** To complete a single, well-defined task from the execution plan.

## Workflow

1.  **Receive Task:** Load the current task from `planning/tasks/active_context.md`.
2.  **Verify Understanding:** Read the task description and acceptance criteria. If anything is unclear, flag it for the Planner/Reviewer immediately. Do not guess.
3.  **Locate Relevant Files:** Identify all files that need to be created or modified to complete the task.
4.  **Implement Changes:**
    -   Write new code or modify existing code.
    -   Strictly adhere to `rules/20-coding-standards.md`.
    -   Make small, atomic commits/changes.
5.  **Run Tests:** Execute relevant unit tests (`rules/30-testing-standards.md`) and any other required checks (linting, etc.).
6.  **Update Documentation:** If the changes affect documentation, update it accordingly (`rules/40-documentation-standards.md`).
7.  **Mark Task Complete:** Once all criteria are met and tests pass, mark the task as complete in `planning/tasks/TASKS.md`.
8.  **Submit for Review:** Notify the Reviewer that the task is ready for quality assurance.