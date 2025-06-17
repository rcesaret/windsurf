# Windsurf IMA Implementation: Architecture Document

**Document Version:** 1.0
**Date:** June 18, 2025
**Author:** Cascade AI (Generated)
**Project:** Windsurf Instructional Memory Architecture (IMA) Enhancement - Phases 1-3

## 1. Introduction

### 1.1 Purpose
This document outlines the technical architecture for implementing Phases 1, 2, and 3 of the Windsurf Instructional Memory Architecture (IMA). It serves as a detailed blueprint for development, specifying component designs, technology choices, data models, interaction patterns, and data flows. Its primary goal is to guide the Cascade AI agent and human developers in constructing a robust, scalable, and maintainable IMA system. This architecture document complements the `windsurf_project_PRD.md`, which defines *what* needs to be built, by detailing *how* these requirements will be technically realized.

### 1.2 Scope
This architecture covers all features and technical components specified within Phases 1 (Foundation Hardening), 2 (Memory & Context Optimization), and 3 (Developer Experience & Usability) as detailed in the `Unified Optimization Blueprint v2.md` and the `windsurf_project_PRD.md`. While it provides high-level design considerations, it excludes exhaustive, line-by-line implementation algorithms unless they are critical to understanding a core architectural decision. Features designated for Phase 4 (Security, Governance & Advanced Features) and items listed in Appendix A (Deferred Features) of the `Unified Optimization Blueprint v2.md` are explicitly out of scope for this architectural specification.

### 1.3 Guiding Architectural Principles
The design and development of the Windsurf IMA will adhere to the following core architectural principles to ensure a high-quality, resilient, and future-proof system:

*   **Modularity:** Components will be designed with well-defined responsibilities and clear, stable interfaces. This promotes separation of concerns, simplifies development and testing, and allows for independent evolution of parts of the system.
*   **Extensibility:** The architecture will be designed to facilitate future enhancements, including the straightforward addition of new features, rules, tools, or even the integration of Phase 4 requirements. Hooks, plugin points, and configurable behaviors will be favored where appropriate.
*   **Maintainability:** Emphasis will be placed on producing clean, well-documented code. Automated testing at various levels (unit, integration) will be integral to the development process. Clear logging and straightforward debugging paths are essential.
*   **Scalability:** Data storage mechanisms (e.g., for memories, rules) and processing components (e.g., indexing, search) will be designed to handle a growing volume of information and increasing operational load without significant performance degradation.
*   **Technology Choices:** Preference will be given to stable, mature, and well-supported open-source technologies. Python and Node.js are the primary languages for tooling, with SQLite for local database needs. Standard libraries and established frameworks will be used to reduce custom development and leverage community support.
*   **Configuration-Driven:** System behavior will be driven by external configurations (primarily YAML or JSON files) where appropriate. This allows for greater flexibility, easier customization by end-users, and modifications without code changes.
*   **Automation:** Automation is a cornerstone of this architecture. GitHub Actions will be leveraged extensively for continuous integration (CI) tasks such as validation, testing, memory indexing, and documentation deployment, reducing manual effort and ensuring consistency.

## 2. Overall System Architecture

### 2.1 Conceptual Overview
The Windsurf IMA is envisioned as an integrated suite of components residing primarily within the `.windsurf/` directory of a project. These components work in concert to guide and enhance the capabilities of an AI agent like Cascade. The IMA interacts with several key entities:

*   **The AI Agent (Cascade):** The primary consumer and interactor with the IMA. The AI reads rules, instructions, and context; utilizes tools; and updates memories based on its operational mode and task.
*   **Version Control System (Git/GitHub):** The IMA's configuration, rules, and tools are version-controlled. GitHub Actions provide CI/CD capabilities.
*   **Integrated Development Environment (IDE), primarily VS Code:** Developers interact with IMA files and tools through the IDE, benefiting from specialized extensions and integrations.

**Key Subsystems:**

1.  **Rule & Instruction Management:** Defines the AI's core behaviors, operational protocols, and mode-specific guidance. Includes `modes.yaml`, rule files, and instruction files.
2.  **Task & Plan Management:** Structures how the AI receives, processes, and executes tasks. Involves `TASKS.md` for backlog management and `.plan.md` templates for detailed execution strategies, along with `active_context.md` for focused task input.
3.  **Memory & Knowledge Management:** Handles the storage, indexing, retrieval, and synthesis of learned information and project-specific knowledge. Includes memory files, knowledge base articles, the SQLite index, and associated tools.
4.  **CLI Tooling Suite:** A set of command-line interface tools (`windsurf`) to automate IMA setup, validation, task progression, and other routine operations.
5.  **CI/CD Automation Pipelines:** GitHub Actions workflows for validating rules, testing tools, indexing memories, and deploying documentation.
6.  **Developer Experience (DX) Enhancement:** Features focused on making the IMA easier for human developers to use, customize, and maintain, including IDE integration and automated documentation.

*(A conceptual diagram illustrating these subsystems and their primary interactions would be beneficial here. For instance, a block diagram showing the AI Agent at the center, with arrows indicating data/control flow to/from the `.windsurf/` directory (broken down into its key subdirectories like `rules/`, `tools/`, `db/`), GitHub (for CI/CD and versioning), and the IDE.)*

### 2.2 Core Directory Structure (`.windsurf/`)
The `.windsurf/` directory serves as the root for all IMA components. Its structure is designed for clarity and logical grouping of functionalities. The `windsurf init` command will establish this baseline structure.

*   **`.windsurf/config/`**: Contains global configuration files for the IMA.
    *   `modes.yaml`: Defines the AI's operational modes, transitions, and associated permissions. This is central to controlling AI behavior.
*   **`.windsurf/rules/`**: Stores Markdown files defining the core, project-agnostic behavioral rules and protocols for the AI. These rules govern interaction, decision-making, and adherence to IMA principles.
    *   `01-meta-rules.md`: High-level rules about IMA usage, mode adherence, etc.
    *   `02-memory-access.md`: Rules for how the AI should query and utilize its memory systems.
    *   `50-version-control.md`: Rules and documented practices for version control and CI/CD within the IMA context.
*   **`.windsurf/instructions/`**: Contains Markdown files with mode-specific operational instructions, guiding the AI's behavior when in a particular state (e.g., `architect-mode.md`, `planner-mode.md`).
    *   `customization-guide.md`: A comprehensive guide for users on how to adapt and extend the IMA.
*   **`.windsurf/memories/`**: Persisted knowledge artifacts generated by or for the AI.
    *   `lessons_learned.md`: Records of insights gained from previous tasks or errors.
    *   `error_documentation.md`: Documentation of common errors and their resolutions.
    *   `synthesized_insights.md`: AI-generated summaries of recurring themes from other memory files.
*   **`.windsurf/knowledge/`**: Stores project-specific domain knowledge, documentation, or reference materials that the AI can consult.
*   **`.windsurf/planning/`**: Manages task execution and planning.
    *   `tasks/TASKS.md`: The master task backlog in Digital TMP format.
    *   `plans/PLAN.template.md`: A template for detailed, multi-stage execution plans for complex tasks.
    *   `active_context.md`: An AI-generated file providing focused context for the current task.
*   **`.windsurf/tools/`**: Houses CLI scripts and supporting utility modules (Python or Node.js).
    *   `cli.py` or `cli.js`: The main `windsurf` CLI entry point.
    *   `rule-lint.py` or `rule-lint.js`: The rule validation tool.
    *   `index_memory.py`: Script for indexing memory files into the SQLite database.
    *   `search_memory.py`: Script for searching the indexed memory.
    *   `docs_generator.py`: Script for generating the documentation site.
*   **`.windsurf/db/`**: Contains local databases used by the IMA.
    *   `memory_index.db`: The SQLite database storing the full-text search index for memories.
*   **`.windsurf/docs/`**: (Potentially) Source files and configuration for the automated documentation site generator (e.g., `mkdocs.yml`, custom theme assets if MkDocs is used).
*   **`.windsurf/templates_for_init/`**: A directory containing the baseline version of all essential IMA files and subdirectories. The `windsurf init` command copies this structure to a new project to bootstrap the IMA.
    *   **`.windsurf/templates_for_init/.vscode/`**: Pre-configured VS Code settings, snippets, and extension recommendations tailored for IMA development, to be copied into the user's project `.vscode/` directory or used as a reference.

## 3. Phase 1: Foundation Hardening - Architecture
Phase 1 focuses on establishing a stable and reliable core for the IMA. This involves formalizing operational protocols, automating validation, standardizing task management, and providing initial tooling to improve developer interaction.

### 3.1 Formalize Operational Mode State Machine
This component ensures predictable and auditable AI behavior by defining clear operational states and transitions.

*   **Component:** `.windsurf/config/modes.yaml`
    *   **Data Model:** A YAML file structured as specified in the PRD. Each mode definition includes:
        *   `name`: Unique identifier for the mode (e.g., `ARCHITECT`, `PLANNER`).
        *   `description`: Human-readable explanation of the mode's purpose.
        *   `allowed_actions`: List of actions or tool categories permissible in this mode.
        *   `forbidden_actions`: List of actions explicitly disallowed.
        *   `entry_conditions`: Criteria that must be met to enter this mode.
        *   `exit_conditions`: Criteria for exiting this mode.
        *   `valid_transitions`: List of modes that can be transitioned to from the current mode.
    *   **Interaction:** This file is read by the AI's core logic to determine its current capabilities and valid next states. It is also referenced by meta-rules for enforcement and can be validated by the `rule-lint` tool (or a dedicated schema validator) as part of the `windsurf validate` command.
*   **Component:** `.windsurf/rules/01-meta-rules.md`
    *   **Integration:** This rule file will contain instructions for the AI to strictly adhere to the `modes.yaml` specification. The AI's internal rule engine or decision-making process must consult `modes.yaml` (or an internal representation of it) when considering a mode change or performing actions, ensuring that transitions are valid and actions are permitted within the current mode.

### 3.2 Continuous Rule Validation Pipeline
This pipeline automates the validation of IMA rule and instruction files, ensuring their integrity and consistency.

*   **Component:** `rule-lint` CLI Tool (`.windsurf/tools/rule-lint.py` or `.js`)
    *   **Technology Stack Choice:** Python is recommended due to its strong text processing capabilities and rich ecosystem of libraries for parsing and analysis (e.g., `PyYAML` for front-matter, `markdown-it-py` or `CommonMark` for Markdown AST parsing, `NLTK` for more advanced linguistic checks if needed). Node.js with libraries like `js-yaml`, `markdown-it`, and `yargs` is a viable alternative.
    *   **Modular Design:** The tool will be structured internally with clear separation of concerns:
        *   **`FileWalker`:** Scans specified directories (`.windsurf/rules/`, `.windsurf/instructions/`) for `.md` files.
        *   **`MarkdownParser`:** Parses Markdown files, making the content and structure (headings, links, code blocks) and YAML front-matter accessible.
        *   **`ValidatorRegistry` & `ValidatorModules`:** A registry will manage individual validator modules. Each module will be responsible for a specific check (e.g., `FrontMatterValidator`, `HeadingStructureValidator`, `AtReferenceValidator`, `WeakLanguageValidator`, `BrokenLinkValidator`). This allows for easy addition of new validation rules.
        *   **`IssueReporter`:** Formats and outputs validation results (e.g., human-readable console output, JSON for IDE integration, or a checkstyle XML format for CI tools).
    *   **Configuration:** An optional configuration file (e.g., `.rule-lintrc.yml` or command-line arguments) will allow users to customize behavior, such as:
        *   Severity levels for different checks (error, warning, info).
        *   Enabling/disabling specific validator modules.
        *   Defining custom dictionaries for weak/strong language.
        *   Specifying custom front-matter schema elements.
*   **Component:** GitHub Action (`.github/workflows/rule-lint.yml`)
    *   **Workflow Triggers:** The workflow will be triggered on `push` events for all branches and `pull_request` events targeting `main` or `develop` branches, specifically when files matching `.windsurf/**/*.md` are modified.
    *   **Workflow Steps:**
        1.  `actions/checkout@vX`: Checks out the repository code.
        2.  `actions/setup-python@vX` (or `actions/setup-node@vX`): Sets up the required runtime environment.
        3.  `Install Dependencies`: Installs any necessary libraries for `rule-lint` (e.g., via `pip install -r requirements.txt` or `npm install`).
        4.  `Run rule-lint`: Executes the `rule-lint` script, potentially with flags to output results in a CI-friendly format (e.g., JSON or annotations).
        5.  `Report Status`: The action will use the exit code of `rule-lint` to determine success or failure. For PRs, this can be integrated as a status check.
    *   **Status Badge Integration:** The workflow can be configured to update a status badge (e.g., using a service like shields.io by pushing a JSON file to a gist or another branch, or by using specialized GitHub Actions that create/update badges).

### 3.3 Adopt Digital TMP Task & Plan System
This standardizes task and plan formats for machine readability and consistent AI processing.

*   **Component:** `.windsurf/planning/tasks/TASKS.md`
    *   **Data Model:** Adheres to Digital TMP v2.3. Each task is a Markdown section with a YAML front-matter block containing structured metadata (e.g., `id`, `title`, `status`, `priority`, `dependencies`, `context_files`, `acceptance_criteria`). The body of the task uses Markdown for detailed description.
    *   **Parsing Logic:** The `windsurf next-task` CLI command will contain the logic to parse this file. This involves reading the Markdown, splitting it into individual task sections, parsing the YAML front-matter for each, and then applying logic to determine the next actionable task.
*   **Component:** `.windsurf/planning/plans/PLAN.template.md`
    *   **Structure:** A Markdown template with predefined H2 sections: `1. Context Ingestion & Analysis`, `2. Preparation & Resource Gathering`, `3. Step-by-Step Execution Plan`, `4. Validation & Verification`, `5. Retrospective & Learning`. Each section will have placeholder prompts to guide the AI or human in filling it out.
    *   **Usage:** This template is intended to be copied (manually or by a future AI-driven planning step) and populated for complex tasks that require detailed, multi-stage planning before execution.

### 3.4 Introduce Basic CLI Tooling & Improve DX
This suite of CLI tools automates common IMA tasks, reducing manual effort and improving developer experience.

*   **Component:** `windsurf` CLI (`.windsurf/tools/cli.py` or `.js`)
    *   **Technology Stack Choice:** Python with `click` or `argparse` is well-suited for robust CLI development. Node.js with `commander` or `yargs` is also a strong option, especially if other tools are Node.js-based.
    *   **Main Entry Point:** The `cli.py` (or `cli.js`) file will serve as the main entry point, using the chosen CLI library to define commands, arguments, and options.
    *   **Sub-command Architecture:** Each command will be implemented as a distinct function or module imported by the main CLI script.
        *   **`init` command:**
            *   **Logic:** Uses file system operations (e.g., Python's `shutil.copytree` or Node.js's `fs-extra.copy`) to duplicate the contents of `.windsurf/templates_for_init/` into the current project's `.windsurf/` directory. It will check if `.windsurf/` already exists to prevent accidental overwrites, perhaps offering a `--force` option. Placeholder replacement logic will scan copied files for predefined tokens (e.g., `{{PROJECT_NAME}}`) and substitute them with values gathered from user prompts or arguments.
            *   **Interaction:** Primarily file system I/O. May prompt the user for project-specific information if not provided via arguments.
        *   **`next-task` command:**
            *   **Logic:** Reads and parses `.windsurf/planning/tasks/TASKS.md` (as described in 3.3). Implements logic to filter tasks by status (e.g., 'pending', 'ready'), sort by priority, and check for unmet dependencies. Once the next task is identified, its status in `TASKS.md` is updated (e.g., to 'in_progress', requiring careful file manipulation to update only the relevant task's front-matter). It then constructs the `.windsurf/planning/active_context.md` file by concatenating task details, content of `context_files`, and potentially initial memory search results (see Phase 2).
            *   **Interaction:** File I/O for `TASKS.md` and `active_context.md`. May invoke `search_memory.py` as a subprocess.
        *   **`validate` command:**
            *   **Logic:** Acts as an orchestrator. It will execute other validation scripts/tools as subprocesses. This includes invoking `rule-lint` and could extend to validating `modes.yaml` against a JSON schema, or `TASKS.md` against the Digital TMP schema if dedicated validators are developed.
            *   **Interaction:** Subprocess execution (e.g., Python's `subprocess.run` or Node.js's `child_process.spawn`). Aggregates results and exit codes from called tools to provide a consolidated report.
    *   **Helper Utilities:** A `utils` module within `.windsurf/tools/` might contain common functions for: YAML parsing/dumping, robust file reading/writing, logging setup, and string manipulation, shared across the CLI commands and other tools.
*   **Component:** `.windsurf/templates_for_init/`
    *   **Structure:** This directory will contain a complete, well-structured baseline `.windsurf/` directory, including example rule files, instruction templates, an empty `TASKS.md` (or one with an initial setup task), the `PLAN.template.md`, an empty `memories/` directory, etc. This ensures that `windsurf init` provides a functional starting point.
*   **Component:** `integration_issue_stubs.md` (Project Root)
    *   **Format:** A Markdown file containing pre-formatted GitHub issue templates. Each template will use Markdown and GitHub's issue template syntax (e.g., YAML front-matter for labels, assignees, and input fields if using GitHub's issue forms) to guide users in reporting bugs or suggesting enhancements related to specific IMA components or integrations.

## 4. Phase 2: Memory & Context Optimization - Architecture
Phase 2 focuses on enhancing the IMA's ability to manage, retrieve, and utilize its knowledge base efficiently. This is critical for scalability and maintaining high-quality AI context.

### 4.1 Implement Robust Memory Indexing & Search
This system enables fast and relevant retrieval of information from the IMA's memory stores.

*   **Component:** SQLite Database (`.windsurf/db/memory_index.db`)
    *   **Schema Design:** A primary table, `memories`, will store metadata and content. Key columns include:
        *   `id` (TEXT, PRIMARY KEY): Unique identifier, possibly derived from file path or front-matter ID.
        *   `file_path` (TEXT, NOT NULL): Absolute or relative path to the source `.md` file.
        *   `title` (TEXT): Title from front-matter or inferred from H1.
        *   `summary` (TEXT): Summary from front-matter or AI-generated if applicable.
        *   `keywords` (TEXT): JSON array of strings representing extracted keywords or tags.
        *   `full_cleaned_content` (TEXT): The main textual content, stripped of Markdown formatting for effective searching.
        *   `created_date` (TEXT): Creation date from front-matter or file system.
        *   `last_modified_date` (TEXT): Last modification date of the source file.
        *   `last_indexed_ts` (TEXT, ISO8601 DATETIME): Timestamp of when the entry was last indexed.
    *   **FTS5 Integration:** A virtual table (e.g., `memories_fts`) will be created using SQLite's FTS5 extension, linked to the `memories` table. This FTS5 table will index `title`, `summary`, `keywords`, and `full_cleaned_content` to enable efficient full-text searching with ranking capabilities (e.g., BM25).
*   **Component:** `index_memory.py` (`.windsurf/tools/`)
    *   **Technology Stack:** Python, `sqlite3` library, `PyYAML` (for front-matter), a Markdown parsing library (e.g., `markdown-it-py` to extract text content, or `BeautifulSoup` if converting to HTML first), and a basic NLP library or custom logic for keyword extraction (e.g., TF-IDF from `scikit-learn`, or simpler regex/frequency-based methods for common terms, excluding a configurable list of stop words).
    *   **Core Logic:**
        1.  **File Discovery:** Recursively scan `.windsurf/memories/` and `.windsurf/knowledge/` for `.md` files.
        2.  **Content Extraction & Cleaning:** For each file, parse YAML front-matter. Extract main textual content, stripping Markdown syntax (e.g., convert to plain text).
        3.  **Keyword Extraction:** Apply keyword extraction logic to the cleaned content and/or front-matter tags.
        4.  **Database Interaction:** Connect to `memory_index.db`. For each file, check if it needs indexing/re-indexing (e.g., based on `last_modified_date` vs `last_indexed_ts`). Insert new entries or update existing ones in the `memories` table and subsequently the `memories_fts` table.
    *   **Error Handling:** Implement robust error handling for file I/O issues, parsing errors, and database transaction failures. Log errors clearly.
*   **Component:** `search_memory.py` (`.windsurf/tools/`)
    *   **Technology Stack:** Python, `sqlite3` library.
    *   **Core Logic:**
        1.  **Argument Parsing:** Accept a query string and optional filters (e.g., date range, specific keywords from a `--tags` argument, source directory like `memories` or `knowledge`).
        2.  **Query Construction:** Build an FTS5 query using the input string. This may involve query sanitization, adding wildcards, or using FTS5 advanced syntax for phrase searching or column-specific weighting if desired. Filters will be translated into standard SQL WHERE clauses applied to the main `memories` table, joined with the FTS results.
        3.  **Database Interaction:** Execute the search query against `memory_index.db`.
        4.  **Result Processing & Formatting:** Retrieve top N matching entries. Results should include file path, title, summary, and potentially a relevant snippet from `full_cleaned_content` where the search terms matched (FTS5's `snippet` function can be used). Output results in a structured format (e.g., JSON for tool chaining, or a human-readable summary for direct CLI use).
*   **CI/CD Integration (e.g., `.github/workflows/memory_management.yml` or as part of `ci.yml`):
    *   **Workflow Trigger:** On pushes to `main` or `develop` branches that modify files within `.windsurf/memories/**/*.md` or `.windsurf/knowledge/**/*.md`.
    *   **Workflow Steps:**
        1.  Checkout repository.
        2.  Setup Python environment and install dependencies.
        3.  Run `python .windsurf/tools/index_memory.py`.
        4.  Commit and push the updated `memory_index.db` file if changes were made. Alternatively, the database could be stored as a build artifact and downloaded by other processes, but committing it simplifies local search consistency.

### 4.2 Implement Context Optimization Strategies
These strategies aim to manage the AI's context window effectively, maximizing information relevance while respecting token limits.

*   **Context Pruning Rule Architecture (AI-Interpreted):**
    *   **Implementation Locus:** This is primarily a behavioral rule enforced by the AI's internal logic, guided by instructions in relevant mode files (e.g., `.windsurf/instructions/architect-mode.md`). No new standalone tool is architected for this specific rule beyond the AI's inherent summarization capabilities.
    *   **Configuration:** The token threshold for triggering summarization (e.g., 10,000 tokens as per PRD) should ideally be a configurable parameter. This could reside in a global IMA configuration file (e.g., `.windsurf/config/ima_settings.yaml`) or be a parameter within the rule/instruction itself if the AI can parse and apply it.
    *   **Process Flow:**
        1.  AI, operating in a mode like ARCHITECT or PLANNER, identifies a large document for ingestion.
        2.  AI estimates token count (or relies on a utility if provided).
        3.  If threshold is exceeded, AI performs a targeted summarization pass, focusing on extracting information pertinent to the current task (entities, decisions, constraints, requirements).
        4.  AI uses this summary as primary context, noting the summarization and retaining a reference to the full document for deeper dives if necessary.

*   **Memory Compaction Workflow (`MAINTENANCE` Mode):**
    *   **Component:** `.windsurf/memories/synthesized_insights.md`
        *   **Structure:** A Markdown file with clear, hierarchical headings. Content should be concise, actionable insights, with explicit references (e.g., by ID or file path and heading) back to the original entries in `lessons_learned.md` or `error_documentation.md` from which they were derived.
    *   **Process Architecture (AI-Driven Task):**
        1.  The `MAINTENANCE` mode is defined in `.windsurf/config/modes.yaml` with permissions to read/write memory files and invoke `search_memory.py` and `index_memory.py`.
        2.  A recurring task (e.g., `P2.M1.S1 - Monthly Memory Compaction`) in `TASKS.md` initiates this workflow.
        3.  The AI, in `MAINTENANCE` mode:
            *   Uses `search_memory.py` or direct reads to access `error_documentation.md` and `lessons_learned.md`.
            *   Applies its analytical capabilities to identify recurring themes, common error patterns, or frequently effective solutions.
            *   Generates or updates `synthesized_insights.md` with these findings.
            *   Ensures this new/updated file is subsequently indexed by `index_memory.py` (either by invoking it or by relying on the CI/CD pipeline if the file is committed).
    *   **Integration:** The `.windsurf/rules/02-memory-access.md` rule will be updated to guide the AI to consult `synthesized_insights.md` as a primary source for common issues or established best practices before performing broader searches.

*   **`active_context.md` Generation (Enhancement to `windsurf next-task` CLI command):**
    *   **Data Sources Integration:** The `windsurf next-task` script will be enhanced to gather information from:
        *   The selected task's full definition in `TASKS.md`.
        *   The content of files explicitly listed in the task's `context_files` metadata.
        *   The top N (e.g., 1-2) most relevant results from `search_memory.py`, using keywords extracted from the task's description or title.
    *   **Logic Flow within `next-task`:**
        1.  Identify next task (as per existing Phase 1 logic).
        2.  Extract task details and `context_files` list.
        3.  For each context file:
            *   Estimate its size. If very large (exceeding a configurable threshold), attempt a basic summarization (e.g., extract first N lines, or use a helper script if a more sophisticated non-AI summarizer is developed; full AI summarization might be too slow for a CLI response). Otherwise, include full content.
        4.  Construct a search query from task keywords. Execute `search_memory.py` as a subprocess and capture its (JSON) output.
        5.  Assemble all gathered information into a structured Markdown file: `.windsurf/planning/active_context.md`. This file should clearly delineate sections for task details, context file contents, and memory search results.
    *   **Output:** A well-formatted `.windsurf/planning/active_context.md` file that the AI can use as its primary input for starting the task.

### 4.3 Enhance CI/CD and Version Control Practices
This section details the architecture for improving the robustness of CI/CD pipelines and formalizing version control for the IMA.

*   **Matrix Testing in CI (e.g., `.github/workflows/ci.yml`):
    *   **Strategy Definition:** The `strategy.matrix` key in the GitHub Actions workflow file will define combinations of:
        *   `os`: e.g., `[ubuntu-latest, windows-latest]`
        *   `python-version`: e.g., `['3.8', '3.9', '3.10', '3.11']` (as applicable to Python tools)
        *   `node-version`: e.g., `['16.x', '18.x', '20.x']` (as applicable to Node.js tools)
    *   **Test Execution Steps:** Within the job, steps will use the matrix variables (e.g., `${{ matrix.os }}`, `${{ matrix.python-version }}`) to set up the environment and run tests. Tests for CLI tools should cover basic invocation, argument parsing, and expected output/exit codes for key scenarios. For scripts like `index_memory.py` or `search_memory.py`, unit tests (e.g., using Python's `unittest` or `pytest`) should verify their core logic with mock data or a temporary test database.
*   **Branch Protection Rules Documentation (`.windsurf/rules/50-version-control.md`):
    *   **Content Focus:** This document will not just list the rules but explain *how* they are technically enforced in GitHub. This may include:
        *   Screenshots or detailed descriptions of the branch protection settings in the GitHub repository for `main` and `develop` branches (e.g., "Require pull request reviews before merging," "Require status checks to pass before merging," "Require conversation resolution before merging," "Dismiss stale pull request approvals when new commits are pushed").
        *   Explanation of which status checks are mandatory (e.g., `rule-lint`, matrix tests).
*   **Semantic Commit/PR Labeling:**
    *   **Tooling (Optional but Recommended):** `commitlint` can be integrated using a GitHub Action like `commitlint-github-action`.
        *   **Configuration:** A `.commitlintrc.js` (or `.yml`/`.json`) file at the project root will define the commit message convention (e.g., `@commitlint/config-conventional`).
        *   **CI Integration:** The action runs on `pull_request` events, checking all commits in the PR against the defined convention. Failure prevents merging if set as a required check.
    *   **PR Label Guidance:** `CONTRIBUTING.md` and/or `50-version-control.md` will provide examples and encourage the use of standardized PR labels (e.g., `type: feat`, `type: fix`, `scope: cli`, `scope: rules`) to aid in changelog generation and project management.

## 5. Phase 3: Developer Experience & Usability - Architecture
Phase 3 focuses on significantly lowering the barrier to entry for using and customizing the Windsurf IMA, and providing developers with tools and resources that streamline their interaction with the AI and the IMA framework.

### 5.1 Comprehensive Onboarding & Customization Guide
This aims to provide new users with a smooth onboarding experience and clear, actionable guidance on tailoring the IMA to their specific project needs.

*   **Component:** Enhanced `windsurf init` CLI command
    *   **Technology Stack:** Python with a library like `questionary` or `inquirerpy` for interactive prompts. Node.js could use `inquirer`.
    *   **Interactive Setup Flow:**
        1.  **Prompting:** The `init` command will guide the user through a series of questions:
            *   Project Name (e.g., `My Awesome Project`)
            *   Primary Programming Language (e.g., `Python`, `JavaScript`, `Java`)
            *   Target AI Model (if known, e.g., `Cascade-Next`, `GPT-4`)
            *   Brief Project Description.
        2.  **Placeholder Replacement:** The script will iterate through files in the copied `templates_for_init/` directory (specifically designated template files, perhaps with a `.tpl` extension or identified by content). It will replace predefined placeholders (e.g., `{{PROJECT_NAME}}`, `{{PRIMARY_LANGUAGE}}`, `{{AI_MODEL}}`, `{{PROJECT_DESCRIPTION}}`) with the user-provided values. This could be simple string replacement or use a templating engine like Jinja2 (for Python) or Handlebars (for Node.js) for more complex substitutions.
        3.  **Git Initialization (Optional):** Check if the current directory is a Git repository. If not, offer to run `git init`.
        4.  **Guidance Output:** Conclude by printing clear next steps to the console, such as: "IMA initialized successfully! Next steps: 1. Review and customize rules in `.windsurf/rules/`. 2. Consult the customization guide: `.windsurf/instructions/customization-guide.md`. 3. Define your first task in `.windsurf/planning/tasks/TASKS.md`."
    *   **Configuration File (`.windsurf/project_context.yaml` or similar):** The information gathered during `init` can be stored in a dedicated YAML file within `.windsurf/config/`. This file can then be referenced by other IMA tools or by the AI itself to understand the project's context without hardcoding it into multiple rule files.

*   **Component:** `.windsurf/instructions/customization-guide.md`
    *   **Structure and Content:** This document will be meticulously structured for clarity and ease of navigation.
        *   **Introduction:** Purpose of the IMA, benefits of customization.
        *   **Core Concepts:** Brief explanation of rules, instructions, memories, operational modes, task management.
        *   **Directory Deep Dive:** Section for each key directory (`.windsurf/config/`, `.windsurf/rules/`, `.windsurf/instructions/`, `.windsurf/memories/`, `.windsurf/planning/`, `.windsurf/tools/`) explaining its purpose and the typical files within, with links to more detailed sections where applicable.
        *   **Customization Walkthroughs (Scenario-Based):**
            *   "Adapting for a [Python Web App] Project": Example rules for linting, testing, common libraries.
            *   "Configuring for a [Data Science] Workflow": Example instructions for data exploration, model training modes.
            *   "Tailoring for a [Technical Writing] Task": Example rules for style guides, terminology.
        *   **Writing Effective IMA Artifacts:**
            *   Rules: Clarity, atomicity, use of `@` refs, avoiding ambiguity.
            *   Instructions: Persona definition, mode-specific guidance, tool usage examples.
            *   Memories: Structure, keyword usage, when to create synthesized insights.
        *   **Using IMA CLI Tools:** Detailed usage instructions for `windsurf init`, `windsurf next-task`, `windsurf validate`, `search_memory.py`, etc., with examples.
        *   **Extending the IMA:** Guidance on adding new `rule-lint` checks, creating new CLI sub-commands, or integrating project-specific scripts.
        *   **Troubleshooting:** Common issues and how to resolve them.
        *   **Best Practices:** Maintaining consistency, version control for IMA files, regular review and refinement.
    *   **Maintainability:** The guide itself should be subject to `rule-lint` checks for broken links or outdated references, ensuring its own quality.

### 5.2 Advanced IDE Integration (VS Code Focus)
To provide developers with a seamless and productive experience when working with IMA files directly within their Integrated Development Environment (IDE), with an initial focus on Visual Studio Code (VS Code).

*   **Component:** VS Code Extension (Conceptual Design)
    *   **Technology Stack:** TypeScript/JavaScript for the extension itself. TextMate grammars (`.tmLanguage.json` or `.tmLanguage.yaml`) for syntax highlighting. Language Server Protocol (LSP) for advanced features like inline linting.
    *   **Development Approach:** Iterative, starting with simpler features and progressing to more complex LSP-based integration.
    *   **Feature Breakdown:**
        1.  **Syntax Highlighting:**
            *   **Grammar Definition:** Create a TextMate grammar file (e.g., `windsurf-ima.tmLanguage.json`) that defines scopes for IMA-specific Markdown conventions:
                *   YAML front-matter in `.md` files (highlighting keys like `id`, `activation_keywords`, `priority`).
                *   `@` file references (e.g., `@.windsurf/config/modes.yaml`).
                *   Task list syntax in `TASKS.md` (e.g., `P1.W1.A1 [ ] Task Title`).
                *   Special directives or keywords defined in rules (e.g., `MUST`, `SHALL`).
            *   **Contribution:** The extension's `package.json` will contribute this grammar for files matching `.windsurf/**/*.md`.
        2.  **Snippets:**
            *   **Definition:** Create JSON snippet files (e.g., `markdown.json` within the extension, scoped to Markdown language ID or a custom IMA language ID).
            *   **Content:** Snippets for:
                *   New rule file template (with front-matter placeholders).
                *   New instruction file template.
                *   New task entry for `TASKS.md` (Digital TMP format).
                *   Mode definition block for `modes.yaml`.
                *   Common rule constructs (e.g., "IF X THEN Y").
        3.  **Command Palette Integration:**
            *   **Contribution:** Register commands in the extension's `package.json` and implement their handlers in the extension's main activation script.
            *   **Implementation:** Commands will execute the `windsurf` CLI tool (e.g., `python .windsurf/tools/cli.py init`) as a child process using Node.js's `child_process.spawn` or `exec`. Output from the CLI can be displayed in a VS Code output channel or terminal.
            *   **Commands:** `Windsurf IMA: Initialize Project`, `Windsurf IMA: Get Next Task`, `Windsurf IMA: Validate Project`.
        4.  **Inline `rule-lint` Feedback (LSP Approach Recommended):**
            *   **Language Server (`ima-language-server.js` or `.py`):**
                *   This server would adapt the core logic of `rule-lint.py` (or `.js`).
                *   It listens for LSP messages (e.g., `textDocument/didOpen`, `textDocument/didChange`, `textDocument/didSave`).
                *   On relevant events, it re-validates the document content and sends diagnostics (errors/warnings with ranges) back to the client.
                *   Could be implemented in Python and communicate via stdio, or Node.js.
            *   **Language Client (VS Code Extension Part):**
                *   The VS Code extension starts and communicates with the `ima-language-server`.
                *   It forwards document changes to the server and receives diagnostics to display in the editor (underlines, hover messages, Problems panel).
            *   **Alternative (Simpler, Non-LSP):** On document save (`workspace.onDidSaveTextDocument`), the extension could directly invoke `rule-lint` as a CLI tool, parse its output, and then use VS Code APIs to create and display diagnostics. This is less real-time and potentially less efficient than LSP.
*   **Component:** `.vscode/` settings in `templates_for_init/`
    *   **`extensions.json`:** Recommend the Windsurf IMA extension itself (once published) and other helpful extensions (e.g., a good Markdown linter, YAML validator).
        ```json
        {
          "recommendations": [
            // "windsurf.ima-extension-id", // Placeholder for actual extension ID
            "bierner.markdown-preview-github-styles",
            "redhat.vscode-yaml"
          ]
        }
        ```
    *   **`settings.json`:**
        *   Configure file associations if a custom language ID is used for IMA files.
        *   Set default formatter for Markdown/YAML.
        *   Potentially configure settings specific to the IMA extension (e.g., path to `rule-lint` if not bundled, linting debounce time).
        ```json
        {
          "editor.formatOnSave": true,
          "[markdown]": {
            "editor.defaultFormatter": "esbenp.prettier-vscode"
          },
          "[yaml]": {
            "editor.defaultFormatter": "redhat.vscode-yaml"
          }
          // "windsurf.ima.ruleLintPath": ".windsurf/tools/rule-lint.py"
        }
        ```

### 5.3 Automated Documentation Site
To automatically generate a searchable, navigable, and user-friendly online documentation site from the Markdown files within the `.windsurf/` directory, ensuring that IMA documentation is always up-to-date and easily accessible.

*   **Component:** `docs_generator.py` (`.windsurf/tools/`)
    *   **Technology Stack Choice:** Python with MkDocs and the Material for MkDocs theme is a strong choice due to its simplicity, excellent search capabilities, and good aesthetics. Sphinx is another powerful alternative, especially for more complex documentation needs.
    *   **Core Logic (Assuming MkDocs):**
        1.  **Configuration Parsing:** The script will ensure an `mkdocs.yml` file exists (or generate a basic one if missing and instructed to do so). This YAML file defines site name, navigation structure, theme, plugins, etc.
        2.  **File Collection:** The script (or MkDocs itself based on `mkdocs.yml` configuration) will identify Markdown files in the specified `docs_dir` (e.g., a copy of relevant `.windsurf/` subdirectories like `rules/`, `instructions/`, `planning/`, and the `customization-guide.md`). A pre-processing step in `docs_generator.py` might be needed to copy these files into a temporary staging area that MkDocs uses as its `docs_dir` to avoid polluting the live `.windsurf/` directory with MkDocs artifacts or requiring MkDocs to navigate complex exclusion rules.
        3.  **Static Site Generation:** The script will invoke the MkDocs build process (e.g., `mkdocs build`) as a subprocess. MkDocs handles Markdown-to-HTML conversion, theme application, navigation generation, and search index creation.
        4.  **Output:** The static HTML site will be generated in a directory specified in `mkdocs.yml` (typically `site/`).
*   **Component:** Documentation Configuration (`.windsurf/docs/` or project root for `mkdocs.yml`)
    *   **`mkdocs.yml` (Example Structure):**
        ```yaml
        site_name: Windsurf IMA Documentation
        site_url: https://<your-github-username>.github.io/<your-repo-name>/ # Or custom domain
        repo_url: https://github.com/<your-github-username>/<your-repo-name>/
        repo_name: <your-repo-name>
        edit_uri: edit/main/.windsurf/docs_src/ # Assuming docs are staged here for editing links

        theme:
          name: material
          features:
            - navigation.tabs
            - navigation.sections
            - navigation.expand
            - navigation.top
            - search.suggest
            - search.highlight
            - content.code.annotate
            - content.tabs.link
          palette:
            scheme: default # or slate
            primary: indigo
            accent: indigo

        plugins:
          - search
          - mermaid2 # If using Mermaid diagrams

        nav:
          - 'Home': 'index.md'
          - 'Core Concepts': 
            - 'Operational Modes': 'config/modes.md' # Example, assuming modes.yaml is converted/copied to md
            - 'Rule System': 'rules/index.md'
          - 'User Guides':
            - 'Customization Guide': 'instructions/customization-guide.md'
            - 'CLI Usage': 'tools/cli-usage.md'
          - 'Reference':
            - 'Rules': 
              - 'Meta Rules': 'rules/01-meta-rules.md'
              - 'Memory Access': 'rules/02-memory-access.md'
            - 'Instructions': 
              - 'Architect Mode': 'instructions/architect-mode.md'
          # ... more navigation items ...

        markdown_extensions:
          - pymdownx.highlight:
              anchor_linenums: true
          - pymdownx.inlinehilite
          - pymdownx.snippets
          - pymdownx.superfences
          - admonition
          - toc:
              permalink: true
        ```
    *   **`docs_dir` Staging:** The `docs_generator.py` might copy relevant files from the live `.windsurf/` directory (e.g., `rules/`, `instructions/customization-guide.md`) into a temporary directory (e.g., `.windsurf/docs_build_src/`) which is then configured as `docs_dir` in `mkdocs.yml`. This keeps the source IMA files clean and allows for potential pre-processing (e.g., converting `modes.yaml` to a readable `modes.md` for documentation).
*   **Component:** CI/CD Workflow for Documentation Deployment (`.github/workflows/deploy-docs.yml`)
    *   **Workflow Trigger:** On pushes to the `main` branch (or `develop`, or on release tags).
    *   **Workflow Steps:**
        1.  **Checkout Repository:** `actions/checkout@v3`
        2.  **Setup Python:** `actions/setup-python@v3` with a specific Python version.
        3.  **Install Dependencies:** `pip install mkdocs mkdocs-material pymdown-extensions mkdocs-mermaid2-plugin` (or from a `requirements-docs.txt`).
        4.  **Generate Documentation:** Run `python .windsurf/tools/docs_generator.py` (which internally calls `mkdocs build`).
        5.  **Deploy to GitHub Pages:** Use a dedicated GitHub Action like `peaceiris/actions-gh-pages@v3` to deploy the contents of the `site/` directory (or the configured `site_dir` from `mkdocs.yml`) to the `gh-pages` branch, which automatically publishes it.
            ```yaml
            name: Deploy Documentation

            on:
              push:
                branches:
                  - main # Or your default branch

            jobs:
              deploy:
                runs-on: ubuntu-latest
                steps:
                  - uses: actions/checkout@v3
                  - name: Set up Python
                    uses: actions/setup-python@v3
                    with:
                      python-version: '3.x'
                  - name: Install dependencies
                    run: |
                      pip install mkdocs mkdocs-material pymdown-extensions mkdocs-mermaid2-plugin
                      # Or pip install -r .windsurf/tools/requirements-docs.txt
                  - name: Build documentation
                    run: python .windsurf/tools/docs_generator.py # This script should call 'mkdocs build'
                  - name: Deploy to GitHub Pages
                    uses: peaceiris/actions-gh-pages@v3
                    with:
                      github_token: ${{ secrets.GITHUB_TOKEN }}
                      publish_dir: ./site # Or your mkdocs site_dir
            ```

## 6. Cross-Cutting Concerns
These aspects are relevant across multiple components and phases of the IMA.

### 6.1 Error Handling & Logging
*   **CLI Tools (`windsurf` CLI, `rule-lint`, `index_memory.py`, `search_memory.py`, `docs_generator.py`):
    *   **Consistent Exit Codes:** Tools should use standard exit codes (e.g., 0 for success, non-zero for errors). Specific error types can have distinct exit codes if useful for scripting.
    *   **Clear Error Messages:** Errors reported to `stderr` should be user-friendly, indicating what went wrong and, if possible, how to fix it.
    *   **Verbose/Debug Logging:** Implement a `--verbose` or `--debug` flag that increases log output. Python tools should use the standard `logging` module, allowing configurable log levels and output destinations (e.g., console, file).
    *   **Structured Logging (Optional):** For complex tools or if logs are to be consumed by other systems, consider structured logging (e.g., JSON format).
*   **GitHub Actions Workflows:
    *   **Clear Log Output:** Ensure workflow steps produce clear, concise logs. Use GitHub Actions' grouping features (`::group::` and `::endgroup::`) to organize logs for complex jobs.
    *   **Failure Reporting:** Workflows should clearly indicate which step failed and why. Use `actions/upload-artifact` to save detailed logs or error reports if necessary.
*   **AI Agent Interaction (Conceptual):
    *   The AI should be instructed (via rules) to report errors encountered during its operations clearly, referencing specific tools or processes that failed.

### 6.2 Configuration Management
*   **Centralized Configuration:** Key IMA configurations (e.g., `modes.yaml`, `rule-lintrc.json`, paths for memory storage, token thresholds) should be stored in human-readable, version-controllable formats (primarily YAML or JSON).
*   **Schema Validation (Where Critical):** For complex configuration files like `modes.yaml` or a potential global `ima_settings.yaml`, consider implementing schema validation (e.g., using `jsonschema` for Python, or `ajv` for Node.js) as part of the `windsurf validate` command or a CI check. This helps prevent malformed configurations.
*   **Environment Variables (Limited Use):** For sensitive information (e.g., API keys if future phases require them) or for overriding default paths in CI environments, environment variables can be used, but file-based configuration should be preferred for most settings.
*   **Default Values:** Tools should have sensible default configurations, allowing users to only specify overrides.

### 6.3 Security (Preliminary Considerations for Phases 1-3)
While Phase 4 is dedicated to comprehensive security, some preliminary considerations are relevant now:
*   **Input Sanitization (CLI Tools):** Any user input to CLI tools (arguments, file paths) should be handled carefully to prevent command injection or path traversal vulnerabilities, especially if these inputs are used to construct shell commands or file system paths dynamically. Use libraries that handle this safely or validate inputs strictly.
*   **Dependency Management:** Regularly update third-party libraries used in tools (Python packages, Node.js modules) to patch known vulnerabilities. Use tools like `pip-audit` (Python) or `npm audit` (Node.js) and consider integrating them into CI.
*   **GitHub Actions Security:**
    *   **Principle of Least Privilege:** Ensure GitHub Actions workflows have only the necessary permissions. Use the `permissions` key in workflow files to restrict token scopes.
    *   **Third-Party Actions:** Pin third-party actions to specific commit SHAs rather than branches to mitigate risks from compromised actions.
    *   **Secret Management:** Store any secrets (e.g., `GITHUB_TOKEN`) securely using GitHub encrypted secrets.
*   **No Hardcoded Secrets:** Ensure no sensitive information (API keys, passwords) is hardcoded in scripts or configuration files committed to the repository.
*   **File System Access:** IMA tools that write to the file system (e.g., `windsurf init`, `index_memory.py`) should operate within well-defined boundaries (primarily the `.windsurf/` directory) and avoid unintended modifications elsewhere.

## 7. Appendices (Optional Examples)
This section is intended to be populated over time with detailed technical artifacts that support the architectural descriptions. Initially, it may serve as a placeholder for planned content.

### 7.1 Detailed Data Schemas
*   **`modes.yaml` Schema (JSON Schema Example):** A formal JSON schema defining the structure and allowed values for `.windsurf/config/modes.yaml`.
*   **`memory_index.db` DDL:** SQL Data Definition Language statements for creating the `memories` table and its associated FTS5 virtual table.
    ```sql
    -- Example DDL for memories table
    CREATE TABLE IF NOT EXISTS memories (
        id TEXT PRIMARY KEY,
        file_path TEXT NOT NULL,
        title TEXT,
        summary TEXT,
        keywords TEXT, -- JSON array of strings
        created_date TEXT,
        last_modified_date TEXT,
        last_indexed_ts TEXT -- ISO8601 DATETIME
    );

    -- Example DDL for FTS5 virtual table (content stored externally in 'memories' table)
    CREATE VIRTUAL TABLE IF NOT EXISTS memories_fts USING fts5(
        title, 
        summary, 
        keywords, 
        full_cleaned_content, 
        content='memories', 
        content_rowid='rowid'
    );

    -- Triggers to keep FTS table synchronized with memories table
    CREATE TRIGGER IF NOT EXISTS memories_ai AFTER INSERT ON memories BEGIN
      INSERT INTO memories_fts (rowid, title, summary, keywords, full_cleaned_content) 
      VALUES (new.rowid, new.title, new.summary, new.keywords, (SELECT full_cleaned_content FROM memories WHERE rowid = new.rowid));
    END;
    CREATE TRIGGER IF NOT EXISTS memories_ad AFTER DELETE ON memories BEGIN
      DELETE FROM memories_fts WHERE rowid=old.rowid;
    END;
    CREATE TRIGGER IF NOT EXISTS memories_au AFTER UPDATE ON memories BEGIN
      UPDATE memories_fts SET 
        title = new.title, 
        summary = new.summary, 
        keywords = new.keywords, 
        full_cleaned_content = (SELECT full_cleaned_content FROM memories WHERE rowid = new.rowid)
      WHERE rowid=old.rowid;
    END;
    ```
*   **`TASKS.md` Front-Matter Schema (JSON Schema Example):** A formal JSON schema for the YAML front-matter of tasks defined in `.windsurf/planning/tasks/TASKS.md`.

### 7.2 Key Data Flow Diagrams
*   **`rule-lint` Process Flow:** Diagram illustrating how `rule-lint` discovers files, applies validators, and reports results.
*   **Memory Indexing (`index_memory.py`):** Diagram showing data flow from source `.md` files through parsing, keyword extraction, to storage in `memory_index.db`.
*   **Memory Search (`search_memory.py`):** Diagram illustrating query input, FTS5 search execution, and result formatting.
*   **`active_context.md` Generation by `windsurf next-task`:** Diagram showing how task details, context files, and memory search results are aggregated into `active_context.md`.

*(Note: Actual diagrams would be created using a tool like Mermaid, PlantUML, or draw.io and embedded or linked here.)*
