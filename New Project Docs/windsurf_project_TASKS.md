# Windsurf IMA Implementation: Project Task List (Phases 1-3)

**Document Version:** 1.0
**Date:** June 18, 2025
**Author:** Cascade AI (Generated)
**Project:** Windsurf Instructional Memory Architecture (IMA) Enhancement - Phases 1-3

## 0. Introduction

### 0.1 Purpose
This document provides a detailed, atomized list of tasks required to implement Phases 1, 2, and 3 of the Windsurf Instructional Memory Architecture (IMA) enhancement project. It is derived from `windsurf_project_roadmap.md`, `Unified Optimization Blueprint v2.md`, and `windsurf_project_PRD.md`. This task list is intended to be used by the Cascade AI agent and human developers to manage and execute the project work.

### 0.2 Task Format
Each task is defined with the following attributes:
*   **ID:** A unique identifier (e.g., P1.1.1).
*   **Description:** A clear statement of what needs to be done.
*   **Parent Task:** Reference to the higher-level task from the roadmap (e.g., P1.1).
*   **Deliverables:** Specific outputs or results of the task.
*   **Dependencies:** Other task IDs that must be completed before this task can start.
*   **Status:** Current state of the task (e.g., To Do, In Progress, Done, Blocked).

---

## 1. Phase 1: Foundation Hardening

**Objective:** Solidify the IMA's core architecture, automate validation, formalize operational protocols, and improve initial DX.

### P1.1 Formalize Operational Mode State Machine

*   **ID:** P1.1.1
    *   **Description:** Define the schema and content for `modes.yaml` based on `Unified Optimization Blueprint v2.md` (Section 1.1) and `windsurf_project_architecture.md` (Section 3.1).
    *   **Parent Task:** P1.1
    *   **Deliverables:** Complete specification for `modes.yaml`.
    *   **Dependencies:** None
    *   **Status:** To Do
*   **ID:** P1.1.2
    *   **Description:** Create the `modes.yaml` file at `.windsurf/config/modes.yaml` with all defined modes (ARCHITECT, PLANNER, EXECUTOR, REVIEWER, SECURITY, MAINTENANCE), descriptions, allowed/forbidden actions, entry/exit conditions, and valid transitions.
    *   **Parent Task:** P1.1
    *   **Deliverables:** `.windsurf/config/modes.yaml` file.
    *   **Dependencies:** P1.1.1
    *   **Status:** To Do
*   **ID:** P1.1.3
    *   **Description:** Update `.windsurf/rules/01-meta-rules.md` to reference `@.windsurf/config/modes.yaml` and mandate adherence to the defined modes and transition protocols.
    *   **Parent Task:** P1.1
    *   **Deliverables:** Updated `.windsurf/rules/01-meta-rules.md`.
    *   **Dependencies:** P1.1.2
    *   **Status:** To Do

### P1.2 Implement Continuous Rule Validation Pipeline

*   **ID:** P1.2.1
    *   **Description:** Design the `rule-lint` CLI tool, including modular components (Validators, File Walker, Reporter) and validation checks (YAML front-matter, headings, @-refs, weak language, broken links, optional `.rule-lintrc.json` support).
    *   **Parent Task:** P1.2
    *   **Deliverables:** Design specification for `rule-lint` tool.
    *   **Dependencies:** P1.1.2 (for `modes.yaml` schema to validate against if applicable)
    *   **Status:** To Do
*   **ID:** P1.2.2
    *   **Description:** Develop the `rule-lint` CLI tool (e.g., `rule-lint.py` or `.js`) in `.windsurf/tools/`.
    *   **Parent Task:** P1.2
    *   **Deliverables:** Functional `rule-lint` script.
    *   **Dependencies:** P1.2.1
    *   **Status:** To Do
*   **ID:** P1.2.3
    *   **Description:** Create GitHub Actions workflow file (`.github/workflows/rule-lint.yml`) to trigger `rule-lint` on push/PR events for `.windsurf/**/*.md` files.
    *   **Parent Task:** P1.2
    *   **Deliverables:** `.github/workflows/rule-lint.yml`.
    *   **Dependencies:** P1.2.2
    *   **Status:** To Do
*   **ID:** P1.2.4
    *   **Description:** Add status badges for `rule-lint` to the main project `README.md` and `.windsurf/README.md`.
    *   **Parent Task:** P1.2
    *   **Deliverables:** Updated README files with status badges.
    *   **Dependencies:** P1.2.3
    *   **Status:** To Do

### P1.3 Adopt Digital TMP Task & Plan System

*   **ID:** P1.3.1
    *   **Description:** Update `.windsurf/planning/tasks/TASKS.md` with the Digital TMP v2.3 schema definition and example tasks as specified in `Unified Optimization Blueprint v2.md` (Section 1.3).
    *   **Parent Task:** P1.3
    *   **Deliverables:** Updated `.windsurf/planning/tasks/TASKS.md` template.
    *   **Dependencies:** None
    *   **Status:** To Do
*   **ID:** P1.3.2
    *   **Description:** Update `.windsurf/planning/plans/PLAN.template.md` to follow the standard multi-stage format (Context Ingestion, Preparation, Execution, Final Validation) as specified in `Unified Optimization Blueprint v2.md` (Section 1.3).
    *   **Parent Task:** P1.3
    *   **Deliverables:** Updated `.windsurf/planning/plans/PLAN.template.md`.
    *   **Dependencies:** None
    *   **Status:** To Do

### P1.4 Introduce Basic CLI Tooling & Improve DX

*   **ID:** P1.4.1
    *   **Description:** Design the `windsurf` CLI tool, including sub-commands (`init`, `next-task`, `validate`) and their functionalities as per `windsurf_project_architecture.md` (Section 3.4).
    *   **Parent Task:** P1.4
    *   **Deliverables:** Design specification for `windsurf` CLI.
    *   **Dependencies:** P1.1, P1.2, P1.3 (for functionalities `validate` and `next-task` will use)
    *   **Status:** To Do
*   **ID:** P1.4.2
    *   **Description:** Create the `templates_for_init/` directory within `.windsurf/` and populate it with the baseline version of all essential IMA files and directories (including `modes.yaml`, `TASKS.md` template, `PLAN.template.md`, basic rules, `.vscode/settings.json`, `.vscode/extensions.json`).
    *   **Parent Task:** P1.4
    *   **Deliverables:** Populated `.windsurf/templates_for_init/` directory.
    *   **Dependencies:** P1.1.2, P1.3.1, P1.3.2
    *   **Status:** To Do
*   **ID:** P1.4.3
    *   **Description:** Develop the `windsurf init` command. It should copy from `templates_for_init/`, handle interactive prompts for project context (name, lang, AI model), populate placeholders, and store context in `.windsurf/project_context.yaml`.
    *   **Parent Task:** P1.4
    *   **Deliverables:** Functional `windsurf init` command.
    *   **Dependencies:** P1.4.1, P1.4.2
    *   **Status:** To Do
*   **ID:** P1.4.4
    *   **Description:** Develop the `windsurf next-task` command. It should parse `TASKS.md`, apply priority/dependency logic, and generate/update `.windsurf/planning/active_context.md`.
    *   **Parent Task:** P1.4
    *   **Deliverables:** Functional `windsurf next-task` command.
    *   **Dependencies:** P1.4.1, P1.3.1 (for `TASKS.md` format)
    *   **Status:** To Do
*   **ID:** P1.4.5
    *   **Description:** Develop the `windsurf validate` command. It should serve as a master validation command invoking `rule-lint` and other potential schema checks (e.g., for `modes.yaml`, `TASKS.md`).
    *   **Parent Task:** P1.4
    *   **Deliverables:** Functional `windsurf validate` command.
    *   **Dependencies:** P1.4.1, P1.2.2
    *   **Status:** To Do
*   **ID:** P1.4.6
    *   **Description:** If Node.js is used for CLI, create `package.json` in `.windsurf/tools/` to manage dependencies.
    *   **Parent Task:** P1.4
    *   **Deliverables:** `.windsurf/tools/package.json` (if applicable).
    *   **Dependencies:** P1.4.3 or P1.4.4 or P1.4.5 (if Node.js chosen)
    *   **Status:** To Do
*   **ID:** P1.4.7
    *   **Description:** Create `integration_issue_stubs.md` at the project root with ready-to-use GitHub issue templates for common IMA enhancements.
    *   **Parent Task:** P1.4
    *   **Deliverables:** `integration_issue_stubs.md` file.
    *   **Dependencies:** None
    *   **Status:** To Do

### P1.M1 Phase 1 Review & Sign-off
*   **ID:** P1.M1.1
    *   **Description:** Conduct thorough testing of all Phase 1 deliverables: `modes.yaml` functionality, `rule-lint` tool and CI, Digital TMP templates, `windsurf` CLI commands (`init`, `next-task`, `validate`).
    *   **Parent Task:** P1.M1
    *   **Deliverables:** Test plan, execution results, bug reports.
    *   **Dependencies:** All P1.1.x, P1.2.x, P1.3.x, P1.4.x tasks.
    *   **Status:** To Do
*   **ID:** P1.M1.2
    *   **Description:** Review all Phase 1 documentation (updated rules, READMEs, templates) and ensure all acceptance criteria from the PRD are met.
    *   **Parent Task:** P1.M1
    *   **Deliverables:** Review report, sign-off.
    *   **Dependencies:** P1.M1.1
    *   **Status:** To Do

---

## 2. Phase 2: Memory & Context Optimization

**Objective:** Enhance memory management, retrieval, and context utilization efficiency.

### P2.1 Implement Robust Memory Indexing & Search

*   **ID:** P2.1.1
    *   **Description:** Design the SQLite database schema for `.windsurf/db/memory_index.db`, including the `memories` table and FTS5 virtual table (`memories_fts`) with triggers for synchronization.
    *   **Parent Task:** P2.1
    *   **Deliverables:** SQL DDL and schema documentation.
    *   **Dependencies:** None
    *   **Status:** To Do
*   **ID:** P2.1.2
    *   **Description:** Develop the `index_memory.py` script in `.windsurf/tools/`. It should scan memory/knowledge files, parse Markdown and front-matter, clean text, extract keywords (e.g., TF-IDF), and populate/update the SQLite DB.
    *   **Parent Task:** P2.1
    *   **Deliverables:** Functional `index_memory.py` script.
    *   **Dependencies:** P2.1.1
    *   **Status:** To Do
*   **ID:** P2.1.3
    *   **Description:** Develop the `search_memory.py` script in `.windsurf/tools/`. It should accept queries, connect to the DB, perform ranked FTS5 search, and return structured results (JSON/human-readable with snippets).
    *   **Parent Task:** P2.1
    *   **Deliverables:** Functional `search_memory.py` script.
    *   **Dependencies:** P2.1.1
    *   **Status:** To Do
*   **ID:** P2.1.4
    *   **Description:** Integrate `index_memory.py` into a CI/CD workflow (e.g., `.github/workflows/memory_management.yml`) to run on changes to `.windsurf/memories/` or `.windsurf/knowledge/` files, committing the updated DB.
    *   **Parent Task:** P2.1
    *   **Deliverables:** CI/CD workflow file and configuration.
    *   **Dependencies:** P2.1.2
    *   **Status:** To Do
*   **ID:** P2.1.5
    *   **Description:** Update `.windsurf/rules/02-memory-access.md` to instruct the AI to prioritize `search_memory.py` for information retrieval.
    *   **Parent Task:** P2.1
    *   **Deliverables:** Updated `.windsurf/rules/02-memory-access.md`.
    *   **Dependencies:** P2.1.3
    *   **Status:** To Do

### P2.2 Implement Context Optimization Strategies

*   **ID:** P2.2.1
    *   **Description:** Add a context pruning rule to AI instruction files (e.g., `architect-mode.md`, `planner-mode.md`) for summarizing large documents based on a configurable token threshold.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Updated instruction files.
    *   **Dependencies:** None
    *   **Status:** To Do
*   **ID:** P2.2.2
    *   **Description:** Define the `MAINTENANCE` operational mode in `.windsurf/config/modes.yaml` with appropriate permissions.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Updated `modes.yaml`.
    *   **Dependencies:** P1.1.2
    *   **Status:** To Do
*   **ID:** P2.2.3
    *   **Description:** Define a recurring task in `TASKS.md` (e.g., `P2.M1.S1 - Monthly Memory Compaction`) for the `MAINTENANCE` mode to generate/update `.windsurf/memories/synthesized_insights.md`.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Updated `TASKS.md` (project's main task list, not the template).
    *   **Dependencies:** P2.2.2, P1.3.1 (for understanding `TASKS.md` structure)
    *   **Status:** To Do
*   **ID:** P2.2.4
    *   **Description:** Ensure `index_memory.py` indexes `synthesized_insights.md`.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Confirmation or update to `index_memory.py` logic.
    *   **Dependencies:** P2.1.2, P2.2.3
    *   **Status:** To Do
*   **ID:** P2.2.5
    *   **Description:** Update `.windsurf/rules/02-memory-access.md` to suggest checking `synthesized_insights.md` first for common issues.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Updated `.windsurf/rules/02-memory-access.md`.
    *   **Dependencies:** P2.2.3, P2.1.5
    *   **Status:** To Do
*   **ID:** P2.2.6
    *   **Description:** Enhance the `windsurf next-task` CLI command (from P1.4.4) to include top `search_memory.py` results in the generated `active_context.md`.
    *   **Parent Task:** P2.2
    *   **Deliverables:** Updated `windsurf next-task` command.
    *   **Dependencies:** P1.4.4, P2.1.3
    *   **Status:** To Do

### P2.3 Enhance CI/CD and Version Control Practices

*   **ID:** P2.3.1
    *   **Description:** Implement matrix testing in the primary CI workflow (e.g., `.github/workflows/ci.yml`) for IMA CLI tools and core scripts across multiple Node.js/Python versions and OS (Linux, Windows).
    *   **Parent Task:** P2.3
    *   **Deliverables:** Updated CI workflow file.
    *   **Dependencies:** P1.2.2, P1.4.x (CLI tools), P2.1.2, P2.1.3
    *   **Status:** To Do
*   **ID:** P2.3.2
    *   **Description:** Create/Update `.windsurf/rules/50-version-control.md` to document branch protection rules enforced in GitHub for `main` and `develop` branches (PR reviews, status checks).
    *   **Parent Task:** P2.3
    *   **Deliverables:** `.windsurf/rules/50-version-control.md`.
    *   **Dependencies:** None
    *   **Status:** To Do
*   **ID:** P2.3.3
    *   **Description:** Update `CONTRIBUTING.md` (create if not exists) and `.windsurf/rules/50-version-control.md` to mandate semantic commit messages (Conventional Commits) and recommend standardized PR labels.
    *   **Parent Task:** P2.3
    *   **Deliverables:** Updated `CONTRIBUTING.md` and `50-version-control.md`.
    *   **Dependencies:** P2.3.2
    *   **Status:** To Do
*   **ID:** P2.3.4
    *   **Description:** (Optional) Implement a GitHub Action (e.g., `commitlint`) to enforce semantic commit message format on PRs.
    *   **Parent Task:** P2.3
    *   **Deliverables:** CI check for commit messages.
    *   **Dependencies:** P2.3.3
    *   **Status:** To Do

### P2.M1 Phase 2 Review & Sign-off
*   **ID:** P2.M1.1
    *   **Description:** Test memory indexing, search, context optimization features (pruning, compaction, active_context generation).
    *   **Parent Task:** P2.M1
    *   **Deliverables:** Test plan, execution results, bug reports.
    *   **Dependencies:** All P2.1.x, P2.2.x tasks.
    *   **Status:** To Do
*   **ID:** P2.M1.2
    *   **Description:** Verify CI/CD enhancements (matrix testing, commit linting if implemented) and version control documentation.
    *   **Parent Task:** P2.M1
    *   **Deliverables:** Review report, sign-off.
    *   **Dependencies:** All P2.3.x tasks, P2.M1.1
    *   **Status:** To Do

---

## 3. Phase 3: Developer Experience & Usability

**Objective:** Lower entry barrier, provide powerful tools, and streamline developer interaction with IMA.

### P3.1 Comprehensive Onboarding & Customization Guide

*   **ID:** P3.1.1
    *   **Description:** Enhance `windsurf init` (from P1.4.3) with a more robust interactive setup flow (using libraries like `inquirer`/`questionary`/`inquirerpy`) for project info, placeholder population in templates, optional Git init, and storing info in `.windsurf/project_context.yaml`.
    *   **Parent Task:** P3.1
    *   **Deliverables:** Enhanced `windsurf init` command.
    *   **Dependencies:** P1.4.3
    *   **Status:** To Do
*   **ID:** P3.1.2
    *   **Description:** Create `.windsurf/instructions/customization-guide.md`. This guide must detail IMA structure, file purposes, customization for different projects/personas, writing effective rules/instructions/memories, CLI usage, and maintenance best practices.
    *   **Parent Task:** P3.1
    *   **Deliverables:** `.windsurf/instructions/customization-guide.md`.
    *   **Dependencies:** All P1 and P2 tools/features it will document.
    *   **Status:** To Do

### P3.2 Advanced IDE Integration (VS Code Focus)

*   **ID:** P3.2.1
    *   **Description:** Develop/Adapt VS Code TextMate grammar (`.tmLanguage.json`) for syntax highlighting of IMA-specific Markdown conventions (@-refs, rule front-matter, `TASKS.md` syntax).
    *   **Parent Task:** P3.2
    *   **Deliverables:** TextMate grammar file.
    *   **Dependencies:** P1.1.2, P1.3.1 (for syntax definitions)
    *   **Status:** To Do
*   **ID:** P3.2.2
    *   **Description:** Create a set of VS Code snippets (JSON format) for common IMA constructs (new rule, task entry, mode definition).
    *   **Parent Task:** P3.2
    *   **Deliverables:** VS Code snippets file(s).
    *   **Dependencies:** P1.1.2, P1.3.1
    *   **Status:** To Do
*   **ID:** P3.2.3
    *   **Description:** Integrate `windsurf` CLI commands into the VS Code Command Palette by registering commands that execute the CLI via `child_process` or similar.
    *   **Parent Task:** P3.2
    *   **Deliverables:** VS Code extension component for command palette integration.
    *   **Dependencies:** P1.4.x (functional `windsurf` CLI)
    *   **Status:** To Do
*   **ID:** P3.2.4
    *   **Description:** Design and implement inline `rule-lint` feedback in VS Code. Preferred approach: Language Server Protocol (LSP). Language Server adapts `rule-lint` logic; Language Client (VS Code extension part) displays diagnostics.
    *   **Parent Task:** P3.2
    *   **Deliverables:** VS Code extension component for inline linting.
    *   **Dependencies:** P1.2.2 (`rule-lint` tool)
    *   **Status:** To Do
*   **ID:** P3.2.5
    *   **Description:** Package the VS Code features (syntax highlighting, snippets, command palette, linting) into a cohesive VS Code extension.
    *   **Parent Task:** P3.2
    *   **Deliverables:** Packaged VS Code extension (e.g., `.vsix` file or ready for marketplace publishing).
    *   **Dependencies:** P3.2.1, P3.2.2, P3.2.3, P3.2.4
    *   **Status:** To Do
*   **ID:** P3.2.6
    *   **Description:** Update `.windsurf/templates_for_init/.vscode/extensions.json` to recommend the new IMA VS Code extension and other useful extensions (Markdown, YAML). Update `.windsurf/templates_for_init/.vscode/settings.json` with initial configurations for formatters or IMA extension settings.
    *   **Parent Task:** P3.2
    *   **Deliverables:** Updated `.vscode/extensions.json` and `.vscode/settings.json` in `templates_for_init/`.
    *   **Dependencies:** P3.2.5, P1.4.2
    *   **Status:** To Do

### P3.3 Automated Documentation Site

*   **ID:** P3.3.1
    *   **Description:** Develop the `docs_generator.py` script in `.windsurf/tools/` using a static site generator (MkDocs + Material theme recommended). Script should parse `mkdocs.yml`, stage relevant `.windsurf/` files, and invoke the build process.
    *   **Parent Task:** P3.3
    *   **Deliverables:** Functional `docs_generator.py` script.
    *   **Dependencies:** Content from P1, P2, P3.1 (e.g., `customization-guide.md`)
    *   **Status:** To Do
*   **ID:** P3.3.2
    *   **Description:** Create and configure `mkdocs.yml` (or equivalent) in `.windsurf/docs/` (or root if preferred by SSG). Define site name, navigation structure, theme, plugins (search, mermaid2 if used).
    *   **Parent Task:** P3.3
    *   **Deliverables:** `mkdocs.yml` configuration file.
    *   **Dependencies:** P3.3.1 (choice of SSG)
    *   **Status:** To Do
*   **ID:** P3.3.3
    *   **Description:** Create CI/CD workflow (`.github/workflows/deploy-docs.yml`) to trigger on `main` branch pushes, build the site using `docs_generator.py`, and deploy the output (e.g., `site/`) to GitHub Pages.
    *   **Parent Task:** P3.3
    *   **Deliverables:** `.github/workflows/deploy-docs.yml`.
    *   **Dependencies:** P3.3.1, P3.3.2
    *   **Status:** To Do

### P3.M1 Phase 3 Review & Sign-off & Project Completion
*   **ID:** P3.M1.1
    *   **Description:** Test comprehensive onboarding (`windsurf init`), VS Code IDE integration features, and the automated documentation site.
    *   **Parent Task:** P3.M1
    *   **Deliverables:** Test plan, execution results, bug reports.
    *   **Dependencies:** All P3.1.x, P3.2.x, P3.3.x tasks.
    *   **Status:** To Do
*   **ID:** P3.M1.2
    *   **Description:** Conduct final project review covering all phases. Ensure all PRD acceptance criteria are met. Obtain stakeholder approval.
    *   **Parent Task:** P3.M1
    *   **Deliverables:** Final review report, project sign-off.
    *   **Dependencies:** P1.M1, P2.M1, P3.M1.1
    *   **Status:** To Do
