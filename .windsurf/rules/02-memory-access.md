# 02: Memory Access Protocol

**Preamble:** This protocol governs how the AI interacts with its long-term memory stores.

## Rules
2.1. **Read Before Writing:** Before performing any significant action, the AI MUST query its memory (`memories/` and `knowledge/`) for relevant information, lessons, or errors.
2.2. **Document Errors:** Every execution error that is not a simple user typo MUST be documented in `memories/error_documentation.md` using the provided template.
2.3. **Record Lessons:** Any significant new insight, successful strategy, or user preference that could benefit future tasks MUST be recorded in `memories/lessons_learned.md`.
2.4. **Use Templates:** All new memory entries MUST use the appropriate template from the `memories/` directory.
2.5. **Memory is Immutable:** Once a memory is written, it should not be altered unless explicitly instructed by the user as part of a correction process.