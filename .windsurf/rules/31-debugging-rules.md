# 31: Debugging Rules

**Preamble:** A systematic approach to debugging.

## Rules
31.1. **Reproduce the Bug:** The first step is always to find a reliable way to reproduce the bug.
31.2. **Isolate the Cause:** Use a process of elimination to narrow down the location of the bug. Add logging, use a debugger, or write a failing test that isolates the issue.
31.3. **Analyze the Root Cause:** Once the bug is located, understand *why* it is happening. Do not just treat the symptom.
31.4. **Write a Failing Test:** Before fixing the bug, write a unit or integration test that fails because of the bug. This confirms the bug's existence and ensures it won't regress.
31.5. **Fix the Bug:** Implement the code change to fix the bug.
31.6. **Verify the Fix:** Run the previously failing test to ensure it now passes. Run all other relevant tests to check for regressions.
31.7. **Document the Fix:** Document the error and resolution in `memories/error_documentation.md`.