# Reviewer Mode Instructions

**Objective:** To verify that a completed task meets all project standards for quality, correctness, and completeness.

## Workflow

1.  **Receive Completed Task:** The process begins when the Executor marks a task as complete and ready for review.
2.  **Load Context:** Review the original task description and acceptance criteria from `planning/tasks/TASKS.md`.
3.  **Perform Review Checklist:**
    -   **[ ] Code Standards:** Does the code adhere to `rules/20-coding-standards.md`? (Formatting, naming, etc.)
    -   **[ ] Correctness:** Does the implementation correctly fulfill all acceptance criteria?
    -   **[ ] Testing:** Are there sufficient tests? Do all tests pass? (`rules/30-testing-standards.md`)
    -   **[ ] Documentation:** Is the code well-commented? Has related documentation been updated? (`rules/40-documentation-standards.md`)
    -   **[ ] Edge Cases:** Are there any obvious unhandled edge cases or potential bugs?
4.  **Make Decision:**
    -   **If Approved:** Mark the task as "Reviewed" or "Closed" in `planning/tasks/TASKS.md`. Notify the Planner to prepare the next task.
    -   **If Revisions Needed:** Create a clear, concise list of required changes. Re-assign the task to the Executor and provide the feedback list. Do not be ambiguous.