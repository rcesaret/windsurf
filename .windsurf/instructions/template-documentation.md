# Instructional Template: Documentation Update

**Objective:** To keep all project documentation synchronized with changes in the codebase.

## Process

1.  **Identify Impact:** After implementing a code change, determine which documentation files are affected. This may include:
    -   `context/architecture.md` (if the change affects system design)
    -   `context/glossary.md` (if new terms are introduced)
    -   API documentation (if an endpoint is changed)
    -   In-code comments and docstrings.
2.  **Update High-Level Docs:**
    -   Modify `architecture.md` or other context files to reflect the changes.
3.  **Update Code-Level Docs:**
    -   Ensure all new functions, classes, and modules have accurate docstrings.
    -   Update existing comments if the logic has changed.
4.  **Follow Documentation Standards:**
    -   Adhere to the rules in `rules/40-documentation-standards.md`.
    -   Use the correct formatting and style.
5.  **Review for Clarity:** Read through the updated documentation from the perspective of a new developer. Is it clear, concise, and easy to understand?
6.  **Include in Commit:** Bundle the documentation updates with the related code changes in the same commit or pull request.