# Issue Stubs – Claude Integration Actions

> These GitHub issue drafts outline the remaining work for integrating external review recommendations. Copy each block into your issue tracker.

---

## VS Code Extension Scaffold
- **Title:** VS Code Extension – Windsurf Snippets & Commands
- **Labels:** tooling, DX, enhancement
- **Description:**
  - Provide snippet completions for `/mode` commands and memory templates.
  - Add command palette items: "Windsurf: Open active_context", "Windsurf: Next Task".
  - Show rule compliance diagnostics.
- **Acceptance Criteria:**
  - Extension published to open VSIX for internal testing.
  - README with installation steps.

---

## Git Hooks Bundle
- **Title:** Pre-commit Hook Bundle (Black, Ruff, Detect-Secrets, Rule-Lint)
- **Labels:** CI/CD, quality, security
- **Description:**
  - Script sets up pre-commit config installing formatters and the new rule-lint.
  - Documentation added to `instructions/customization-guide.md`.
- **Acceptance Criteria:**
  - Hook passes on clean repo, fails when rule numbering broken.

---

## Metrics Dashboard & Nightly Workflow
- **Title:** Analytics Dashboard – Rule Compliance & Memory Growth
- **Labels:** analytics, DevOps
- **Description:**
  - Create `workflows/metrics.yml` to run nightly.
  - Store aggregated metrics in `metrics/metrics.db`.
  - Generate `metrics/README.md` dashboard using Shields.io badges.
- **Acceptance Criteria:**
  - Workflow passes and commits updated dashboard.
  - README badges display latest metrics.

