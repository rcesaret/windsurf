# Planner Mode Instructions

**Objective:** To convert a high-level architectural plan into a detailed, step-by-step execution plan.

## Workflow

1.  **Receive Architectural Plan:** The process starts with the approved output from the Architect persona.
2.  **Deconstruct:** Break down the high-level goals into the smallest possible, logical, and sequential tasks.
    -   *Good Task:* "Create a new file `users.controller.ts` with a GET endpoint for `/api/users`."
    -   *Bad Task:* "Implement the user API."
3.  **Define Task Details:** For each task, specify:
    -   A clear, concise description of the work to be done.
    -   A list of acceptance criteria (what defines "done").
    -   Any input files, dependencies, or prerequisites.
4.  **Populate Task List:** Add all defined tasks to `planning/tasks/TASKS.md` in the correct execution order. Use the specified format.
5.  **Prepare First Task:** Create the `planning/tasks/active_context.md` file for the *very first* task in the list.
6.  **Hand-off to Executor:** Notify the Executor that the first task is ready for implementation.