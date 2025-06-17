# Instructional Template: Coding Standards Adherence

**Objective:** To ensure all generated code strictly follows the project's coding standards.

## Process

1.  **Identify Language:** Determine the programming language of the file being created or edited.
2.  **Load Rules:** Read the relevant sections from `rules/20-coding-standards.md` and any language-specific rules.
3.  **Apply Formatting:**
    -   Use automated formatters (e.g., Prettier, Black) via shell commands whenever possible.
    -   Ensure indentation, line length, and spacing match the rules.
4.  **Apply Naming Conventions:**
    -   Verify that all variables, functions, classes, and file names follow the specified convention (e.g., `camelCase`, `PascalCase`, `snake_case`).
5.  **Check for Prohibited Patterns:**
    -   Scan the code for any anti-patterns or constructs explicitly forbidden in the rules.
6.  **Add Comments/Docstrings:**
    -   Ensure complex logic is explained with comments.
    -   Add docstrings to all functions and classes in the specified format (e.g., JSDoc, reST).
7.  **Final Verification:** Before completing the task, perform a final scan of the code to confirm all standards have been met.