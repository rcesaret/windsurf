# 30: Testing Standards

**Preamble:** These rules define the standards for automated testing.

## Rules
30.1. **Test Coverage:** All new business logic (services, utility functions) MUST be accompanied by unit tests. New API endpoints must have integration tests.
30.2. **Test Location:** Tests MUST be located in the `/tests` directory and mirror the source file structure. (e.g., `src/api/utils.ts` is tested by `tests/api/utils.spec.ts`).
30.3. **Test Structure (Arrange-Act-Assert):** Tests should be structured clearly:
    1.  **Arrange:** Set up the test environment and inputs.
    2.  **Act:** Execute the function or code being tested.
    3.  **Assert:** Check that the outcome is as expected.
3.4. **Mock Dependencies:** Unit tests MUST mock all external dependencies (e.g., databases, external APIs) to ensure they are testing the unit in isolation.
30.5. **Descriptive Names:** Test descriptions should clearly and concisely state what they are testing. (e.g., `it('should return a user when given a valid ID')`).