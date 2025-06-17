# 50: Version Control (Git) Rules

**Preamble:** Rules for using Git for version control.

## Rules
50.1. **Branching Model:** Use a feature-branching model. All new work must be done on a dedicated branch, not directly on `main`.
    - Branch names should be descriptive (e.g., `feature/user-csv-export`, `bugfix/login-error`).
50.2. **Commit Messages:** Commit messages MUST follow the Conventional Commits specification.
    - Format: `type(scope): description`
    - Example: `feat(api): add endpoint for user data export`
    - Example: `fix(web): correct typo on login button`
50.3. **Atomic Commits:** Each commit should represent a single, logical change. Avoid large, monolithic commits.
50.4. **Pull Requests (PRs):** All branches must be merged into `main` via a Pull Request. The PR must be reviewed and approved before merging.
50.5. **Rebase Before Merge:** Before merging a PR, rebase your feature branch on top of the latest `main` to maintain a clean, linear history.