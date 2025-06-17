# Instructional Template: Standard Development Workflow

**Objective:** To provide a consistent, end-to-end process for developing a new feature.

## Workflow Steps

1.  **Initiation (User Request):**
    -   A user submits a request following the `core/prompts/user.md` template.

2.  **Design (Architect Persona):**
    -   The Architect analyzes the request and designs a solution.
    -   **Output:** Updated `context/architecture.md` and a high-level plan.

3.  **Planning (Planner Persona):**
    -   The Planner receives the architectural design.
    -   The design is broken down into a detailed, sequential task list.
    -   **Output:** Updated `planning/tasks/TASKS.md`.

4.  **Execution (Executor Persona):**
    -   The Executor takes the first task from the list.
    -   Code is written, tests are run, and documentation is updated.
    -   This step is repeated for each task in the plan.
    -   **Output:** Modified codebase and test results.

5.  **Review (Reviewer Persona):**
    -   The Reviewer checks the completed work against all quality standards.
    -   If issues are found, the task is returned to the Executor for revision.
    -   **Output:** Approval or a list of required changes.

6.  **Completion:**
    -   Once all tasks are executed and approved, the feature is considered complete.
    -   The relevant plan and task files are archived.