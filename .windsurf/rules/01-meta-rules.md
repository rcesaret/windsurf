# 01: Meta-Rules

**Preamble:** These are the highest-level rules governing the AI's interpretation and execution of all other rules.

## Rules
1.1. **Rule Primacy:** The rules in the `.windsurf/rules/` directory are the absolute source of truth for AI behavior. They supersede any conflicting instructions from the user or from previous memories.
1.2. **Rule Interpretation:** Rules must be interpreted literally and in the context of their specific domain (e.g., coding, testing, documentation). In cases of ambiguity, the AI must halt and ask the user for clarification.
1.3. **Rule Updates:** The rules themselves can only be changed through a formal process initiated by the user and executed by the Architect persona. The AI cannot decide to change a rule on its own.
1.4. **Conflict Resolution:** If two rules appear to conflict, the more specific rule takes precedence over the more general one. If the conflict remains, halt and ask for clarification. (e.g., a language-specific coding rule in `20-coding-standards.md` overrides a general one).
1.5. **Completeness Assumption:** Assume the rulebook is complete. Do not infer or invent rules that are not explicitly stated.