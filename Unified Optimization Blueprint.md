# **Windsurf IMA: Unified Optimization Blueprint (Revision 1.1)**

**Document ID:** WDA-20250617-FINAL-R1.1  
**Version:** 1.1  
**Date:** June 17, 2025  
**Subject:** A refined and detailed implementation blueprint for the phased optimization of the Windsurf Instructional Memory Architecture (IMA). This revision incorporates minor, consistent enhancements identified across all strategic planning documents.

## **1.0 Introduction and Guiding Principles**

This document serves as the definitive technical blueprint for implementing the next iteration of the Windsurf Instructional Memory Architecture (IMA). It synthesizes the pragmatic recommendations from `o3_high_project_review.md`, the enterprise-ready enhancements from the `Strategic Optimization Reports (Parts 1 & 2)`, and the visionary proposals from `Proposal CASCADE ADDITIONS`, all filtered through the phased, user-centric strategy outlined in **`NEW_PATH_FORWARD.md`**.

The guiding principles for this implementation, derived from the approved "Path Forward," are:

1.  **Immediate Value:** Prioritize changes that solve current pain points and deliver tangible benefits quickly.
2.  **Incremental Implementation:** Structure work into phases that build upon each other, allowing for stable, iterative progress.
3.  **Minimal User Burden:** Favor enhancements that automate processes or simplify workflows over those that require significant new learning or configuration from the end-user.
4.  **Positive Performance Impact:** Focus on optimizations that improve system responsiveness, token efficiency, and scalability.
5.  **Maintainability:** Ensure that all changes contribute to a more robust, auditable, and easily managed system in the long term.

This blueprint will detail the specific file creations, modifications, and content required to execute this vision.

---

## **Phase 1: Foundation Hardening**

**Focus:** Solidify the core architecture, automate validation, formalize protocols, and improve the initial user experience to eliminate ambiguity and prevent common errors.

### **1.1 Formalize the Operational Mode State Machine**

* **Objective:** To centralize the definition of AI personas (modes) into a single, machine-readable configuration file, separating configuration from rules and making the state machine explicit and extensible.
* **Rationale:** As identified in `Strategic Optimization Report - Part 1` and `Path Forward for Windsurf IMA Implementation.md`, this formalization provides clarity, predictability, and simplifies the process of customizing or adding new AI personas.
* **Implementation Steps:**

    1.  **Create `config/modes.yaml`:** Create a new file at `.windsurf/config/modes.yaml`.

    2.  **Populate `modes.yaml`:** Add the following content, which defines the core personas and their operational contracts. This structure is inspired by `Claude_Review/advanced_mode_system.txt`.

        ```yaml
        # .windsurf/config/modes.yaml
        # Defines the formal operational modes (personas) for the Cascade agent.
        # This file is the single source of truth for the AI's state machine.

        version: "2.1.0"
        default_mode: "PLANNER"

        modes:
          ARCHITECT:
            description: "High-level system design, strategic planning, and architectural refinement. Focuses on the 'why' and 'how' at a macro level."
            allowed_actions:
              - "read_context"
              - "update_architecture"
              - "create_diagrams"
              - "research_technologies"
              - "make_technical_decisions"
            forbidden_actions:
              - "write_application_code"
              - "execute_tasks"
            entry_requirements:
              - "A new major feature request or architectural change is needed."
            exit_conditions:
              - "The architectural plan is documented and approved by the user."

          PLANNER:
            description: "Detailed task planning and decomposition. Breaks down high-level goals into atomic, executable tasks."
            allowed_actions:
              - "read_architectural_plans"
              - "create_task_lists"
              - "decompose_requirements"
              - "estimate_effort"
              - "define_acceptance_criteria"
            forbidden_actions:
              - "write_application_code"
              - "make_architectural_decisions"
            entry_requirements:
              - "An approved architectural plan is available."
            exit_conditions:
              - "A detailed task plan (`TASKS.md`) is created with all tasks having clear criteria."

          EXECUTOR:
            description: "Task implementation and code generation. Focused on writing, testing, and debugging code according to a specific plan."
            allowed_actions:
              - "write_code"
              - "run_tests"
              - "create_files"
              - "modify_files"
              - "execute_commands"
            forbidden_actions:
              - "modify_task_plans"
              - "make_architectural_decisions"
              - "skip_validation_steps"
            entry_requirements:
              - "An approved task plan (`*.plan.md`) with clear acceptance criteria is available."
            exit_conditions:
              - "All acceptance criteria are met, all tests pass, and the code is ready for review."

          REVIEWER:
            description: "Quality assurance, validation, and documentation finalization. Acts as the gatekeeper for code quality."
            allowed_actions:
              - "review_code"
              - "validate_requirements"
              - "run_quality_checks"
              - "update_documentation"
              - "approve_or_reject_work"
            forbidden_actions:
              - "write_application_code"
              - "modify_requirements"
            entry_requirements:
              - "A task has been completed by the Executor and is ready for review."
            exit_conditions:
              - "The work is approved and merged, or feedback has been provided for revisions."
          
          SECURITY:
            description: "Security-focused analysis, vulnerability scanning, and secure coding review."
            allowed_actions:
              - "read_context"
              - "run_vulnerability_scans"
              - "review_code_for_security"
              - "propose_security_hardening"
            forbidden_actions:
              - "write_application_code"
              - "deploy_changes"
            entry_requirements:
              - "A security review is requested, or CI triggers a security alert."
            exit_conditions:
              - "Security review is complete and a report is generated."

        transitions:
          ARCHITECT:
            valid_next_modes: ["PLANNER"]
          PLANNER:
            valid_next_modes: ["EXECUTOR", "ARCHITECT"]
          EXECUTOR:
            valid_next_modes: ["REVIEWER", "PLANNER"]
          REVIEWER:
            valid_next_modes: ["EXECUTOR", "PLANNER"]
          SECURITY:
            valid_next_modes: ["PLANNER", "EXECUTOR"]
        ```

    3.  **Update Meta-Rules:** Modify `.windsurf/rules/01-meta-rules.md` to reference the new configuration file.

        **Current Snippet (Conceptual):**
        > You MUST operate in one of the following modes: RESEARCH, PLAN, EXECUTE...

        **New Snippet:**
        ```markdown
        ---
        # META-PROTOCOL: AI OPERATIONAL MODES

        - **MANDATE**: You MUST operate in one and only one of the formal operational modes defined in `@.windsurf/config/modes.yaml`.
        - **DECLARATION**: You MUST begin every response by declaring your current mode in the format `[MODE: <MODE_NAME>]`. There are no exceptions to this rule.
        - **TRANSITION**: You CANNOT transition between modes without an explicit user command or upon successful completion of a mode's exit conditions as defined in the configuration.
        - **GUIDANCE**: For detailed definitions and protocols for each mode, you MUST consult the relevant file in the `@.windsurf/instructions/` directory.
        ```

### **1.2 Continuous Rule Validation Pipeline**

* **Objective:** To automate the validation of all rule files to prevent syntax errors, broken references, and logical inconsistencies ("rule drift").
* **Rationale:** As identified in `o3_high_project_review.md`, automated validation is essential for guaranteeing rule integrity and maintaining consistent AI behavior.
* **Implementation Steps:**

    1.  **Create `rule-lint` CLI Tool:** Develop a script (e.g., in Node.js or Python) located at `.windsurf/tools/rule-lint.js`. This script will:
        * Parse all `.md` files in the `.windsurf/rules/` directory.
        * Validate the YAML front-matter against a defined schema (e.g., ensure `activation`, `priority` exist).
        * Check for consistent heading structures and numbering.
        * Verify that any `@` file references point to existing files.
        * Flag the use of weak language ("should", "consider") and suggest assertive language ("MUST", "SHALL").

    2.  **Create GitHub Action for Rule Linting:** Create a new workflow file at `.github/workflows/rule-lint.yml`.

        ```yaml
        # .github/workflows/rule-lint.yml
        name: 'Windsurf Rule Linting'

        on:
          push:
            paths:
              - '.windsurf/**'
          pull_request:
            paths:
              - '.windsurf/**'

        jobs:
          lint-rules:
            runs-on: ubuntu-latest
            steps:
              - name: Checkout Repository
                uses: actions/checkout@v4

              - name: Setup Node.js
                uses: actions/setup-node@v4
                with:
                  node-version: '20'

              - name: Run Rule Linter
                id: rule-lint
                run: node .windsurf/tools/rule-lint.js

              - name: Update Lint Status Badge
                uses: RubbaBoy/BYOB@v1.3.0
                with:
                  badge_path: .github/badges/rule-lint-status.svg
                  label: 'Rule Linting'
                  status: ${{ job.status }}
                  color: ${{ job.status == 'success' ? 'green' : 'red' }}
                  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        ```

    3.  **Add Status Badge to README:** Add the following line to the main project `README.md` to display the linting status.

        ```markdown
        ![Rule Linting Status](.github/badges/rule-lint-status.svg)
        ```

### **1.3 Adopt Digital TMP Task & Plan System**

* **Objective:** To standardize the format for `TASKS.md` and `.plan.md` files to ensure they are machine-readable and contain all necessary context for the AI.
* **Rationale:** The Digital TMP format, as detailed in `TASKS_example.md` and promoted in the strategic reports, provides the structure needed for predictable task execution and automated workflows.
* **Implementation Steps:**

    1.  **Update `planning/tasks/TASKS.md`:** Replace the content of this file with a template that defines and exemplifies the Digital TMP v2.3 schema.

        ```markdown
        ---
        Digital TMP: AI-Driven Task Management Protocol
        version: "2.3"
        last_updated: 2025-06-17
        ---
        # MASTER TASK BACKLOG

        ## TASK SCHEMA (For Reference)
        # Each task MUST adhere to this YAML-like structure.
        # - id: "P1.W1.A1" (Hierarchical ID: Phase.Week.Category.Number)
        #   description: "**Title:** Detailed summary."
        #   status: "pending | in_progress | blocked | done"
        #   depends_on: ["ID1", "ID2"]
        #   context_files: ["path/to/file.md#section"]
        #   deliverables: ["path/to/output.py"]
        #   validation_steps: ["Run pytest on deliverables.", "Confirm output matches schema."]
        #   sub_tasks: [...] (Optional for hierarchical tasks)

        ---

        # 🏗️ Phase 1: Foundation Hardening

        - id: P1.W1.A1
          description: "**Formalize Operational Modes:** Create the `config/modes.yaml` file and migrate mode definitions."
          status: pending
          depends_on: []
          context_files:
            - "NEW_PATH_FORWARD.md#Phase-1-Foundation-Hardening"
            - "Strategic Optimization Report - Part 1.md#2.1.-Formalize-the-Operational-Mode-State-Machine"
          deliverables:
            - ".windsurf/config/modes.yaml"
            - ".windsurf/rules/01-meta-rules.md"
          validation_steps:
            - "Confirm `modes.yaml` exists and is populated."
            - "Confirm `01-meta-rules.md` correctly references `modes.yaml`."

        # (Add other Phase 1 tasks here)
        ```

    2.  **Update `planning/plans/PLAN.template.md`:** Ensure the plan template follows the standard multi-stage format.

        ```markdown
        # Plan: {{TASK_TITLE}}
        tags: [{{TAG_1}}, {{TAG_2}}]
        task_id: {{TASK_ID}}
        description: {{TASK_DESCRIPTION}}
        
        ---
        
        ## Stage 1: Context Ingestion & Verification
        - [ ] **Global Context Review:** Review `@.windsurf/rules/01-meta-rules.md`, `@.windsurf/context/architecture.md`, etc.
        - [ ] **Task-Specific Context Review:** Review all files listed in `context_files` for task `{{TASK_ID}}`.
        - [ ] **Memory Review:** Review `@.windsurf/memories/error_documentation.md` and `@.windsurf/memories/lessons_learned.md`.
        
        ---
        
        ## Stage 2: Preparation
        - [ ] Identify all target file paths from the `deliverables` field.
        - [ ] Confirm all `depends_on` tasks are `done`.
        
        ---
        
        ## Stage 3: Execution
        - [ ] Step 3.1: [Atomic action step]
        - [ ] Step 3.2: [Another atomic action step]
        
        ---
        
        ## Stage 4: Final Validation & Cleanup
        - [ ] Verify each item in the `deliverables` list has been created/modified correctly.
        - [ ] Execute each command listed in the `validation_steps` and confirm success.
        - [ ] Propose the required changes to `TASKS.md` to update the task status to `done`.
        ```

### **1.4 Introduce Basic CLI Tooling & Improve DX**

* **Objective:** To reduce manual effort, prevent common errors, and improve the developer experience by automating routine setup and planning tasks.
* **Rationale:** A simple CLI improves the developer experience and ensures consistency, as recommended in the O3 review and strategic reports.
* **Implementation Steps:**

    1.  **Create `.windsurf/tools/cli.js`:** Develop a simple command-line interface.

        ```javascript
        // .windsurf/tools/cli.js (Conceptual Example)
        const fs = require('fs-extra');
        const path = require('path');
        const { program } = require('commander');

        const templateDir = path.join(__dirname, '../templates_for_init'); // Assume a dir with all base files

        program
          .command('init')
          .description('Initialize a new .windsurf directory in the current project.')
          .action(() => {
            fs.copySync(templateDir, './.windsurf');
            console.log('Successfully initialized .windsurf directory.');
          });

        program
          .command('next-task')
          .description('Generate active_context.md for the next pending task.')
          .action(() => {
            // Logic to parse TASKS.md, find next task, and create active_context.md
            console.log('Generated active_context.md for the next task.');
          });
        
        program.parse(process.argv);
        ```
    2.  **Add `package.json` for Tooling:** Create a `package.json` in `.windsurf/tools/` to manage dependencies like `commander` and `fs-extra`.
    3.  **Create `integration_issue_stubs.md`:** As proposed in the `Claude_Review` synthesis, create a file at the root of the project named `integration_issue_stubs.md` to provide ready-to-use GitHub issue templates for common enhancements, guiding community or team contributions.

        ```markdown
        # Issue Stubs – Windsurf IMA Integration Actions

        > These GitHub issue drafts outline future work for integrating external review recommendations. Copy each block into your issue tracker.

        ---

        ## VS Code Extension Scaffold
        - **Title:** VS Code Extension – Windsurf Snippets & Commands
        - **Labels:** tooling, DX, enhancement
        - **Description:** Provide snippet completions for `/mode` commands, rule templates, and task entries. Add command palette items: "Windsurf: Next Task", "Windsurf: Validate Rules". Show rule compliance diagnostics inline.
        - **Acceptance Criteria:** Extension published to marketplace. README with installation steps.

        ---

        ## Git Hooks Bundle
        - **Title:** Pre-commit Hook Bundle (Black, Ruff, Rule-Lint)
        - **Labels:** CI/CD, quality, security
        - **Description:** Create a script to set up a standard pre-commit config that installs formatters and runs the rule-lint tool.
        - **Acceptance Criteria:** Hook passes on clean repo, fails when rule numbering is broken or code is unformatted.

        ---

        ## Metrics Dashboard & Nightly Workflow
        - **Title:** Analytics Dashboard – Rule Compliance & Memory Growth
        - **Labels:** analytics, DevOps
        - **Description:** Create `workflows/metrics.yml` to run nightly. Store aggregated metrics in `metrics/metrics.db`. Generate `metrics/README.md` dashboard using Shields.io badges.
        - **Acceptance Criteria:** Workflow passes and commits updated dashboard. README badges display latest metrics.
        ```

---

## **Phase 2: Performance and Scalability**

**Focus:** Implement optimizations to ensure the IMA remains fast and efficient as the project's knowledge base and complexity grow.

### **2.1 Implement Memory Indexing System**

* **Objective:** To accelerate the retrieval of relevant information from the `memories/` and `knowledge/` directories.
* **Rationale:** As noted in `o3_high_project_review.md`, a linear scan of memory files will become a performance bottleneck. An index is crucial for scalability.
* **Implementation Steps:**

    1.  **Create Indexing Script:** Develop `.windsurf/tools/index_memory.py`. This script will:
        * Iterate through all `.md` files in `memories/` and `knowledge/`.
        * Extract YAML front-matter (ID, keywords, summary) and main content for each file.
        * Populate a SQLite database (`.windsurf/db/memory_index.db`) with this data, enabling full-text search (FTS5).

    2.  **Create Search Script:** Develop `.windsurf/tools/search_memory.py` that takes a query string and returns the top N matching memory entries from the SQLite index.

    3.  **Integrate into CI/CD:** Add a step to the GitHub Actions workflow that runs `index_memory.py` whenever files in `memories/` or `knowledge/` are changed, keeping the index up-to-date.

    4.  **Update Rules:** Update the `memory-access.md` rule to instruct the AI to use the `search_memory.py` tool for finding relevant lessons or errors instead of reading the entire files.

### **2.2 Implement Context Optimization**

* **Objective:** To intelligently manage the context window to stay within token limits while maximizing relevant information.
* **Rationale:** An advanced IMA must manage the finite resource of context length effectively, as highlighted in `Doc08` and the strategic reports.
* **Implementation Steps:**

    1.  **Add Context Pruning Rule:** Modify `.windsurf/instructions/architect-mode.md` to include a new protocol step:
        > "Before ingesting a document larger than 10,000 tokens, you MUST first perform a summarization pass. Your summary should extract key entities, architectural decisions, and constraints. Use this summary as the primary context for planning."

    2.  **Create Memory Compaction Workflow:** Create `.github/workflows/memory_compaction.yml`. This workflow will run on a schedule (e.g., monthly):
        * It triggers Cascade in a special `MAINTENANCE` mode.
        * Cascade's task is to read `memories/error_documentation.md` and `memories/lessons_learned.md`, identify recurring themes or patterns, and synthesize them into a new, concise file: `memories/insights_summary.md`.
        * The system can then be instructed to prioritize this summary file during context retrieval.

### **2.3 Enhance CI/CD and Version Control**

* **Objective:** To improve the robustness of the CI/CD pipeline and formalize version control practices.
* **Rationale:** This ensures higher quality for all contributions and aligns with professional DevOps standards mentioned in the reviews.
* **Implementation Steps:**

    1.  **Implement Matrix Testing:** Update the primary CI workflow (`deploy-staging.yml` or a new `ci.yml`) to use a build matrix for testing across multiple Node.js or Python versions, as recommended in `o3_high_project_review.md`.
    2.  **Document Branch Protection Rules:** Add an appendix to `.windsurf/rules/50-version-control.md` that explicitly documents the branch protection rules for `main` and `develop` branches (e.g., "Require PR review before merging," "Require status checks to pass").
    3.  **Adopt Semantic Commit/PR Labeling:** Update `CONTRIBUTING.md` and `50-version-control.md` to mandate semantic commit messages (e.g., `feat:`, `fix:`, `docs:`) and PR labels. This aligns with the "quick win" recommendation to improve change traceability.

---

## **Phase 3: Developer Experience & Usability**

**Focus:** Make the system easier and more pleasant to use for end developers and teams. Reduce the learning curve and cognitive load required to maintain the rule system.

### **3.1 Comprehensive Onboarding & Customization Guide**

* **Objective:** To lower the barrier to entry for new users and teams adopting the IMA template.
* **Rationale:** The strategic reviews consistently highlighted that the system's power comes with complexity. A comprehensive guide is essential for adoption.
* **Implementation Steps:**

    1.  **Create Interactive Onboarding Workflow:** Develop an interactive script `tools/onboarding.js` (invoked by `windsurf init`) that guides a new user through a checklist to customize essential files. This script will:
        * Prompt for project name, description, and primary language.
        * Open `context/architecture.md` and `context/technical.md` in the editor and provide instructions on what to fill in.
        * Ask the user to confirm their coding style preferences and update a section in `.windsurf/rules/20-coding-standards.md`.
    2.  **Create a Comprehensive Customization Guide:** Create the file `.windsurf/instructions/customization-guide.md`, adapting the content from `Claude_Review/customization_guide.md`. This guide will provide a clear checklist of which files `MUST`, `SHOULD`, and `CAN` be customized, along with common patterns for different project types (web dev, data science, etc.).

### **3.2 "Windsurf Doctor" Diagnostic Tool**

* **Objective:** To provide users with a self-service tool for validating their IMA configuration and diagnosing common problems.
* **Rationale:** This empowers users to solve their own issues and ensures the IMA remains in a healthy, consistent state.
* **Implementation Steps:**
    1.  **Expand `rule-lint` to `windsurf doctor`:** Evolve the `rule-lint` tool into a more comprehensive `windsurf doctor` command. In addition to linting rules, this command will:
        * Check for the existence of all essential `.windsurf/` files and directories.
        * Validate the `config/modes.yaml` file against a JSON schema to ensure correctness.
        * Perform a "health check" on the memory index to ensure it's up-to-date.
        * Provide clear, actionable error messages and suggestions for fixes.

### **3.3 Enhanced Documentation**

* **Objective:** To make all aspects of the IMA easy to understand and reference.
* **Rationale:** Good documentation is a key component of developer experience.
* **Implementation Steps:**
    1.  **Create a Static Documentation Site:** Set up a simple static site generator (like MkDocs or Docsify) in a `/docs_site` directory. Configure it to build a browsable, searchable website from all the Markdown files in the `.windsurf/` directory.
    2.  **Add `README.md` to All Subdirectories:** Ensure every subdirectory within `.windsurf/` (e.g., `context/`, `instructions/`, `rules/`) has a `README.md` file that clearly explains the purpose of that directory and the files within it. This was a minor but consistent suggestion for improving clarity.
    3.  **Encourage Richer Context Documents:** Modify the template for `context/architecture.md` to encourage versioning and linking to live diagrams, as suggested in `o3_high_project_review.md`.

---

## **Phase 4: Security & Governance**

**Focus:** Augment the system with features that appeal to enterprise and safety-conscious users, without compromising the streamlined core.

### **4.1 Implement Security Persona & Protocols**

* **Objective:** To embed security-first thinking into the AI's workflow.
* **Rationale:** The O3 review and strategic reports all identified a dedicated security mode as a critical enhancement for enterprise readiness.
* **Implementation Steps:**
    1.  **Create Security Rules and Instructions:**
        * Create `.windsurf/rules/60-security-protocol.md`. This rule will contain high-level security checklists (e.g., check for OWASP Top 10 vulnerabilities, scan for hardcoded secrets).
        * Create `.windsurf/instructions/security-mode.md`. This will detail the workflow for the Security persona: threat modeling, running scans, and recommending patches.
    2.  **Add Security Scanning to CI/CD:** Enhance the primary CI workflow to include automated security scanning steps using tools like `trivy` for container images and `CodeQL` for static code analysis. A failing scan will block PR merges and can trigger the Security persona.
    3.  **Add Threat Modeling Template:** Create `.windsurf/templates/threat-model.md` and update the Architect persona's instructions to recommend filling it out for any new feature involving user data or external APIs.

### **4.2 Implement Basic Governance and Observability**

* **Objective:** To create auditable trails for rule changes and provide insights into AI performance.
* **Rationale:** Governance and observability are crucial for managing the AI system at scale and ensuring accountability.
* **Implementation Steps:**
    1.  **Implement Rule Change Governance:** Create the file `.windsurf/rules/00-governance-protocol.md`. This rule will state that all changes to files within `.windsurf/rules/` and `.windsurf/instructions/` MUST be made via a Pull Request that is reviewed by at least one other team member.
    2.  **Create a Simple Audit Log:** Implement a lightweight audit logging mechanism. A GitHub Action can be configured to run on merges to `main` that modify the `.windsurf/` directory. This action will append a summary of the commit message to `.windsurf/logs/audit_trail.md`.
    3.  **Establish Performance Monitoring:**
        * Create a workflow (`.github/workflows/metrics.yml`) that runs nightly to collect basic metrics.
        * This can include: number of rules, size of memory files, number of tasks completed.
        * Store this data in a simple SQLite database at `.windsurf/metrics/metrics.db`.
        * (Stretch Goal) Create a script to generate a `metrics/README.md` dashboard from this database.

---
## **Appendix A: Deferred Features**

To maintain focus on core value and simplicity, the following advanced features proposed in the strategic reviews are explicitly deferred. They will be considered for future implementation phases once the foundational architecture is stable and validated.

* **Multi-Agent Communication Protocol (MACP):** Complex handoff protocols are not required for the current single-agent (mode-switching) paradigm.
* **Business Requirement Traceability:** The overhead of maintaining a traceability matrix is too high for the initial implementation.
* **Immutable Audit Trail (Blockchain Logging):** A standard, version-controlled text log is sufficient for current auditability needs.
* **Dynamic Rule Loading:** Static rule loading based on file globs provides sufficient context management for the current scale.
* **Advanced Ethical Governance Layer:** Core ethical guidelines will be included in `global_rules.md`, but a comprehensive, separate governance framework is deferred.

## **Appendix B: Final Proposed Directory Structure**

The final, optimized directory structure for the Windsurf IMA will be as follows:

```
.
└── .windsurf/
    ├── README.md
    ├── config/
    │   └── modes.yaml
    ├── context/
    │   ├── README.md
    │   ├── architecture.md
    │   ├── directory_structure.md
    │   └── technical.md
    ├── db/
    │   └── memory_index.db
    ├── instructions/
    │   ├── README.md
    │   ├── architect-mode.md
    │   ├── customization-guide.md
    │   ├── executor-mode.md
    │   ├── planner-mode.md
    │   ├── reviewer-mode.md
    │   └── security-mode.md
    ├── knowledge/
    │   └── README.md
    ├── logs/
    │   └── audit_trail.md
    ├── memories/
    │   ├── README.md
    │   ├── error_documentation.md
    │   └── lessons_learned.md
    ├── metrics/
    │   ├── README.md
    │   └── metrics.db
    ├── planning/
    │   ├── README.md
    │   ├── plans/
    │   │   └── PLAN.template.md
    │   └── tasks/
    │       └── TASKS.md
    ├── rules/
    │   ├── 00-governance-protocol.md
    │   ├── 01-meta-rules.md
    │   ├── 02-memory-access.md
    │   └── 60-security-protocol.md
    ├── templates/
    │   ├── README.md
    │   └── threat-model.md
    ├── tools/
    │   ├── cli.js
    │   ├── index_memory.py
    │   └── rule-lint.js
    └── workflows/
        ├── deploy-staging.yml
        └── rule-lint.yml
```