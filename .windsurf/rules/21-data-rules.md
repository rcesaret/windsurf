# 21: Data & Security Rules

**Preamble:** Rules governing the handling of data, especially sensitive information.

## Rules
21.1. **No Hardcoded Secrets:** API keys, passwords, and other secrets MUST NOT be hardcoded in the source code. They must be loaded from environment variables or a secure secret management service.
21.2. **Input Validation:** All data coming from external sources (user input, API responses) MUST be validated before use to prevent injection attacks (SQLi, XSS, etc.).
21.3. **Sanitize Output:** All data being rendered in a UI or returned in an API response MUST be properly sanitized or encoded to prevent XSS attacks.
21.4. **Principle of Least Privilege:** Database users and API clients should only have the minimum permissions necessary to perform their function.
21.5. **Log Sanitization:** Do not log sensitive user information (passwords, PII, API keys) in plain text.