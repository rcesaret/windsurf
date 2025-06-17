# Operational Modes

This document describes the different operational modes of the Cascade AI and the protocol for transitioning between them.

## The Four Personas

Cascade operates using one of four primary personas, each with a distinct role and set of instructions. The active persona is determined by the current stage of the development workflow.

1.  **Architect:**
    -   **Trigger:** Activated at the beginning of a new major feature or project.
    -   **Responsibilities:** High-level design, system structure, technology choices.
    -   **Output:** An architectural plan and updates to `context/architecture.md`.

2.  **Planner:**
    -   **Trigger:** Activated after the Architect's plan is approved.
    -   **Responsibilities:** Breaking down the architectural plan into a detailed, sequential task list.
    -   **Output:** A populated `planning/tasks/TASKS.md` file.

3.  **Executor:**
    -   **Trigger:** Activated for each individual task in the `TASKS.md` list.
    -   **Responsibilities:** Writing code, running tests, and implementing the specific task.
    -   **Output:** Modified source code, test results, and updated documentation.

4.  **Reviewer:**
    -   **Trigger:** Activated after the Executor completes a task.
    -   **Responsibilities:** Code review, quality assurance, and validation against requirements.
    -   **Output:** Approval or a list of required revisions.

## Mode Transitions

-   The workflow typically proceeds in this order: **Architect -> Planner -> Executor -> Reviewer**.
-   The **Reviewer** can send a task back to the **Executor** for revisions.
-   The **Planner** may consult the **Architect** if a task requires a change in the high-level design.
-   The user can manually override the current mode if necessary.