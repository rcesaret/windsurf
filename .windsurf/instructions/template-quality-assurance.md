# Instructional Template: Quality Assurance Checklist

**Objective:** To perform a standardized quality check on all completed work.

## Checklist

### 1. Requirements Validation
-   [ ] Does the implementation meet all acceptance criteria specified in the task?
-   [ ] Does it solve the problem described in the original goal?

### 2. Code Quality
-   [ ] Does the code adhere to `rules/20-coding-standards.md`?
-   [ ] Is the code clean, readable, and maintainable?
-   [ ] Are there any obvious performance issues or anti-patterns?

### 3. Testing
-   [ ] Does the code have adequate test coverage (unit, integration)? (`rules/30-testing-standards.md`)
-   [ ] Do all existing and new tests pass?
-   [ ] Have edge cases been considered and tested?

### 4. Documentation
-   [ ] Is the code sufficiently commented?
-   [ ] Have all relevant external documents (`architecture.md`, etc.) been updated? (`rules/40-documentation-standards.md`)

### 5. Security
-   [ ] Are there any potential security vulnerabilities (e.g., SQL injection, XSS)?
-   [ ] Are secrets and sensitive data handled correctly? (`rules/21-data-rules.md`)