# 32: Quality Assurance Rules

**Preamble:** Rules governing the overall quality assurance process.

## Rules
32.1. **Definition of Done:** A task or feature is not "done" until it has been implemented, tested, documented, and successfully reviewed.
32.2. **Manual Testing:** For user-facing features, a manual testing pass based on the acceptance criteria is required before deployment.
32.3. **Regression Testing:** Before a major release, a full regression test suite must be run to ensure existing functionality has not been broken.
32.4. **Peer Review:** All code MUST be reviewed by at least one other developer (or the Reviewer AI persona) before being merged into the main branch.
32.5. **Continuous Integration:** All commits pushed to the repository MUST trigger a CI pipeline that runs all automated checks (linting, tests, builds). A failing pipeline blocks the change.