# 40: Documentation Standards

**Preamble:** Rules for creating and maintaining project documentation.

## Rules
40.1. **Documentation is Code:** Treat documentation with the same care as source code. It must be clear, accurate, and kept up-to-date.
40.2. **Audience Awareness:** Write for the intended audience. (e.g., `README.md` is for new developers, API docs are for consumers).
40.3. **Docstrings:** All public functions, classes, and modules MUST have docstrings explaining their purpose, parameters, and return values.
40.4. **Architecture Documentation:** The `context/architecture.md` file MUST be updated whenever a change is made to the system's design or structure.
40.5. **Automated Docs:** Where possible, leverage tools to auto-generate documentation from source code comments (e.g., JSDoc, Sphinx).