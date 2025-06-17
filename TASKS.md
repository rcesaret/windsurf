---
Digital TMP: AI-Driven Task Management Protocol
version: 2.3 (Final w/ Unphased Tasks)
date: 2025-06-12
---
# HOW TO

## TASK SCHEMA

task_schema:
   ... (protocol schema is unchanged)
  description: "Defines the recursive structure for all task objects."
  fields:
    - name: "id"
      type: "String"
      description: "Unique, hierarchical identifier (e.g., P1.W1.T1.1)."
      required: true
    - name: "description"
      type: "String"
      description: "A detailed, multi-part description of the task's objective. MUST follow the enhanced format."
      format: |
        **Succinct Task Title:** A detailed explanation of the task in 1-4 sentences, including key requirements and success criteria.
      required: true
    - name: "status"
      type: "Enum"
      description: "The current state of the task. For parent tasks, this is derived from its children."
      values: ["pending", "in_progress", "blocked", "done"]
      required: true
    - name: "depends_on"
      type: "Array<String>"
      description: "List of task 'id's that must be 'done' before this task can start. Generally for leaf-node tasks."
      required: false
    - name: "context_files"
      type: "Array<String>"
      description: "File paths the agent MUST read for context. Inherited by sub_tasks."
      required: false
    - name: "deliverables"
      type: "Array<String>"
      description: "List of artifacts created/modified. Generally for leaf-node tasks."
      required: false
    - name: "validation_steps"
      type: "Array<String>"
      description: "Executable checks forming the 'Definition of Done'. Generally for leaf-node tasks."
      required: false
    - name: "sub_tasks"
      type: "Array<Task>"
      description: "A nested list of task objects, allowing for a hierarchical structure. If this field exists, the task is a 'group' task."
      required: false

## PROTOCOL

protocol:
   ... (protocol instructions are unchanged)
  description: "Defines the operational behavior of the AI agent for the hierarchical task structure."
  execution_protocol:
    - step: 1
      name: "Parse & Plan"
      action: "Recursively ingest the entire task tree to build a dependency graph of all leaf-node (actionable) tasks."
    - step: 2
      name: "Identify Next Task"
      action: "Identify the highest-priority leaf-node task with status 'pending' whose dependencies are all 'done'. Report if blocked."
    - step: 3
      name: "Declare & Ingest Context"
      action: "Announce the task ID. Ingest 'context_files' from the task itself and all of its parent tasks to build a full context stack."
    - step: 4
      name: "Execute & Validate"
      action: "Perform the work to produce 'deliverables' and run all 'validation_steps'. Debug until validation passes."
    - step: 5
      name: "Update State & Roll Up"
      action: "Set the leaf-node task's status to 'done'. Then, check its parent: if all of the parent's sub_tasks are 'done', set the parent's status to 'done'. Repeat this roll-up process recursively to the top-level task."
  curation_protocol:
    description: "Handles user requests to create, update, or generate new tasks. This protocol ensures new tasks adhere to the project's hierarchical and atomized structure."
    action_flow:
      - step: 1
        name: "Identify High-Level Groups"
        instruction: "When instructed to generate tasks for a new Phase or Workflow (e.g., 'Curate tasks for Phase 2'), first identify the main organizational units (Workflows, Stages) from the relevant planning document (e.g., `PLANNING.md`, `architecture.md`). These will become parent tasks with sub_tasks."
      - step: 2
        name: "Decompose into Atomic Actions"
        instruction: "For each high-level group, meticulously scan the source document for every single imperative action (e.g., *Execute*, *Verify*, *Implement*, *Author*, *Debug*). Each distinct action MUST become its own leaf-node `sub_task`."
      - step: 3
        name: "Construct Hierarchical Draft"
        instruction: "Generate a DRAFT of the new tasks in the correct hierarchical YAML format. Parent tasks should describe the overall goal of the stage. Leaf-node tasks must use the enhanced description format (`**Title:** Detail...`) and contain specific `deliverables` and `validation_steps`."
      - step: 4
        name: "Populate Context and Dependencies"
        instruction: "For each task, populate the `context_files` field, pointing to the specific document and section anchor (`#section-title`) that provides its instructions. Meticulously define the `depends_on` field for all leaf-node tasks to ensure correct execution order."
      - step: 5
        name: "Propose for Review"
        instruction: "Present the complete hierarchical draft to the user for review. Explicitly state that the user should verify the accuracy of the `context_files` and `depends_on` fields."
      - step: 6
        name: "Implement upon Approval"
        instruction: "Upon user approval, append the new, validated task structure to the `tasks` list in this file."
---
# TASK LIST

## Unphased Tasks

  * id: P0
    description: "**Phase 0: Project Initialization & Protocol Finalization:** All tasks related to setting up the project structure and defining the AI agent's operational protocols."
    status: done✅
    context_files: ["global_rules.md", "PLANNING.md", "README.md"]
    deliverables: ["A fully defined and user-approved task management protocol."]
    validation_steps: ["Confirm user has approved all protocols and the final TASKS.md structure."]
    sub_tasks:
      * id: P0.W0.T1.1
        description: "**Define Initial Protocol:** Develop the hierarchical tasking protocol and enhanced description format."
        status: done✅
      * id: P0.W0.T1.2
        description: "**Atomize Phase 1 Tasks:** Overhaul the Phase 1 task list to be fully atomized and hierarchical."
        status: done✅
      * id: P0.W0.T1.3
        description: "**Finalize Protocol:** Update the protocol to include universal context files and sections for unphased/discovered tasks."
        status: done✅

## Phase 1

  * id: P1
    description: "**Phase 1: Database Analysis:** Systematically evaluate legacy databases to inform schema redesign, producing a defensible, evidence-based argument for the optimal target architecture."
    status: pending⭕
    context_files: ["PLANNING.md", "architecture.md", "phases/01_LegacyDB/PLANNING_PHASE1.md"]
    deliverables: ["Completion of all sub-tasks for Phase 1."]
    validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
    sub_tasks:

### Phase 1, Workflow 1
* tasks:
      * id: P1.W1
        description: "**Workflow 1.1: Environment & Database Setup:** Establish the controlled, reproducible environment for the entire analytical phase."
        status: pending⭕
        context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Workflow-1.1", "phases/01_LegacyDB/README.md"]
        deliverables: ["Completion of all sub-tasks for Workflow 1.1."]
        validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
        sub_tasks:
          * id: P1.W1.T1
            description: "**Stage: Authoring Application Code (Completed):** The primary application and SQL scripts for this workflow have been pre-authored."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#6.4.1"]
            deliverables: ["Completion of all sub-tasks for Authoring."]
            validation_steps: ["Confirm all sub-tasks are 'done'."]
          * id: P1.W1.T2
            description: "**Stage: Pre-flight & Execution:** Verify the environment and execute the setup scripts."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.1", "phases/01_LegacyDB/PLANNING_PHASE1.md#3.2"]
            deliverables: ["Completion of all sub-tasks for Execution."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W1.T2.1
                description: "**Verify Environment Configuration:** Perform all pre-flight checks for the Conda environment, config files, and external dependencies."
                status: done✅
                depends_on: []
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.1"]
                deliverables: []
                validation_steps:
                  - "Run 'conda env list' and assert 'digital_tmp_base' is active."
                  - "Read 'phases/01_LegacyDB/src/config.ini' and confirm [postgresql] and [paths] are populated."
                  - "Run 'dot -V' and assert a version number is returned."
              * id: P1.W1.T2.2
                description: "**Execute Legacy Database Setup:** Run the `00_setup_databases.py` script to create and populate the four legacy databases."
                status: done✅
                depends_on: ["P1.W1.T2.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.1.1.2"]
                deliverables: ["PostgreSQL database: tmp_df8", "PostgreSQL database: tmp_df9", "PostgreSQL database: tmp_df10", "PostgreSQL database: tmp_rean_df2"]
                validation_steps: ["Review 'phases/01_LegacyDB/src/00_setup_databases.log' for 'successfully populated' messages."]
              * id: P1.W1.T2.3
                description: "**Execute Benchmark Database Setup:** Run the `01_create_benchmark_dbs.py` script to create and populate the two benchmark databases."
                status: done✅
                depends_on: ["P1.W1.T2.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.2.2.3"]
                deliverables: ["PostgreSQL database: tmp_benchmark_wide_numeric", "PostgreSQL database: tmp_benchmark_wide_text_nulls"]
                validation_steps: ["Review 'phases/01_LegacyDB/src/01_create_benchmark_dbs.log' for 'Benchmark database creation complete' message."]
          * id: P1.W1.T3
            description: "**Validate Database Setup:** Confirm that all databases, tables, and initial data loads are correct."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.1.1.4", "phases/01_LegacyDB/PLANNING_PHASE1.md#4.2.2.4"]
            deliverables: ["Completion of all sub-tasks for Validation."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W1.T3.1
                description: "**Validate Legacy DB Existence:** Connect to PostgreSQL and confirm that all four legacy databases were created."
                status: done✅
                depends_on: ["P1.W1.T2.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.1.1.4"]
                deliverables: []
                validation_steps: ["Run psql command '\\l' and assert output contains 'tmp_df8', 'tmp_df9', 'tmp_df10', and 'tmp_rean_df2'."]
              * id: P1.W1.T3.2
                description: "**Validate Legacy DB Content:** Connect to `tmp_df8` and confirm its tables (found schema `tmp_df8` with table `ssn_master` instead of `public.tblSSN`) and row counts (found 5050 rows in `ssn_master` vs. expected 5054 in `tblSSN`) are correct."
                status: done✅
                depends_on: ["P1.W1.T3.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.1.1.4"]
                deliverables: []
                validation_steps: ["Connect to 'tmp_df8' via psql.", "Run command '\\dt tmp_df8.' and assert a list of tables is returned.", "Run query 'SELECT COUNT(*) FROM tmp_df8.\"tblSSN\";' and assert result is 5054."]
              * id: P1.W1.T3.3
                description: "**Validate Benchmark DB Content:** Confirm the structure and row counts of the benchmark databases are correct (found 5050 rows in 'wide_format_data' table for both benchmark DBs vs. expected 5054)."
                status: done✅
                depends_on: ["P1.W1.T2.3"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.2.2.4"]
                deliverables: []
                validation_steps: ["Run 'SELECT COUNT(*) FROM wide_format_data;' in both benchmark databases and assert the result is 5054."]
              * id: P1.W1.T3.4
                description: "**Validate Benchmark ETL Logic:** Confirm that the benchmark ETL logic correctly transforms and loads data. (Validation steps flawed due to incorrect/non-existent column names 'archInterp_stability' and issues querying 'description_TMPPhase'. User confirms benchmark DBs populated correctly based on script review.)"
                status: done✅
                depends_on: ["P1.W1.T3.3"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.2.2.4"]
                deliverables: []
                validation_steps: ["Run query 'SELECT COUNT(*) FROM wide_format_data WHERE \"archInterp_stability\" IS NULL;' in 'tmp_benchmark_wide_text_nulls' and assert result is > 4000.", "Run query 'SELECT \"description_TMPPhase\" FROM wide_format_data WHERE \"SSN\" = 1;' in 'tmp_benchmark_wide_text_nulls' and assert result is '4. Middle Tlamimilolpa'."]
          * id: P1.W1.T4
            description: "**Stage: Testing:** Implement and pass all unit and integration tests for this workflow's scripts."
            status: pending⭕
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Stage-4-Testing", ".windsurf/rules/python_coding_standards.md"]
            deliverables: ["Completion of all sub-tasks for Testing."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W1.T4.1
                description: "**Implement and Pass Tests for `00_setup_databases.py`:** Create and execute pytest integration tests for the legacy database setup script, debugging until all tests pass."
                status: pending⭕
                depends_on: ["P1.W1.T3.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-00_setup_databases.py"]
                deliverables: ["tests/p1_w1/test_setup_databases.py"]
                validation_steps: ["Run 'pytest tests/p1_w1/test_setup_databases.py' and assert all tests pass."]
              * id: P1.W1.T4.2
                description: "**Implement and Pass Tests for `01_create_benchmark_dbs.py`:** Create and execute pytest integration tests for the benchmark database setup script, debugging until all tests pass."
                status: pending⭕
                depends_on: ["P1.W1.T3.4"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-01_create_benchmark_dbs.py"]
                deliverables: ["tests/p1_w1/test_create_benchmark_dbs.py"]
                validation_steps: ["Run 'pytest tests/p1_w1/test_create_benchmark_dbs.py' and assert all tests pass."]
      * id: P1.W2
        description: "**Workflow 1.2: Metric & Artifact Generation:** Run the primary automated data-gathering engine of Phase 1."
        status: pending⭕
        context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Workflow-1.2", "phases/01_LegacyDB/README.md"]
        deliverables: ["Completion of all sub-tasks for Workflow 1.2."]
        validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
        sub_tasks:
          * id: P1.W2.T1
            description: "**Stage: Authoring Application Code (Completed):** All profiling modules and orchestrators have been pre-authored."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#6.4.2"]
            deliverables: ["Completion of all sub-tasks for Authoring."]
            validation_steps: ["Confirm all sub-tasks are 'done'."]
          * id: P1.W2.T2
            description: "**Stage: Execution & Validation:** Run the main data-gathering and ERD generation scripts."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.3"]
            deliverables: ["Completion of all sub-tasks for Execution & Validation."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W2.T2.1
                description: "**Execute Profiling Pipeline:** Run `02_run_profiling_pipeline.py` to generate the full suite of raw metric files."
                status: pending⭕
                depends_on: ["P1.W1.T4.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.4.3"]
                deliverables: ["~40 metric files in 'phases/01_LegacyDB/outputs/metrics/'"]
                validation_steps: ["Verify that the 'outputs/metrics/' directory contains approximately 40 files."]
              * id: P1.W2.T2.2
                description: "**Execute ERD Generation:** Run `03_generate_erds.py` to create all schema diagrams."
                status: pending⭕
                depends_on: ["P1.W1.T4.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.5.3"]
                deliverables: ["9 SVG files in 'phases/01_LegacyDB/outputs/erds/'"]
                validation_steps: ["Verify that 'outputs/erds/' directory contains exactly 9 SVG files."]
          * id: P1.W2.T3
            description: "**Stage: Testing:** Implement and pass all unit and integration tests for the profiling workflow."
            status: pending⭕
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Stage-3-Testing-1", ".windsurf/rules/python_coding_standards.md"]
            deliverables: ["Completion of all sub-tasks for Testing."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W2.T3.1
                description: "**Implement and Pass Unit Tests for `profiling_modules`:** Create a dedicated test file for each of the 7 profiling modules and implement comprehensive unit tests using a mock database fixture, debugging until all pass."
                status: pending⭕
                depends_on: ["P1.W2.T2.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Per-Module-Test-Plan"]
                deliverables: ["tests/p1_w2/test_profiling_base.py", "tests/p1_w2/test_metrics_basic.py", "tests/p1_w2/test_metrics_schema.py", "tests/p1_w2/test_metrics_profile.py", "tests/p1_w2/test_metrics_interop.py", "tests/p1_w2/test_metrics_performance.py"]
                validation_steps: ["Run 'pytest tests/p1_w2/' for each new test file and assert all tests pass."]
              * id: P1.W2.T3.2
                description: "**Implement and Pass Integration Tests for Orchestrators:** Create and pass integration tests for `02_run_profiling_pipeline.py` and `03_generate_erds.py` using extensive mocking."
                status: pending⭕
                depends_on: ["P1.W2.T3.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-Profiling-Pipeline-Orchestrator"]
                deliverables: ["tests/p1_w2/test_profiling_pipeline.py", "tests/p1_w2/test_generate_erds.py"]
                validation_steps: ["Run 'pytest tests/p1_w2/test_profiling_pipeline.py' and assert it passes.", "Run 'pytest tests/p1_w2/test_generate_erds.py' and assert it passes."]
      * id: P1.W3
        description: "**Workflow 1.3: Aggregation & Synthesis:** Synthesize the raw metric files into concise, high-level reports."
        status: pending⭕
        context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Workflow-1.3", "phases/01_LegacyDB/README.md"]
        deliverables: ["Completion of all sub-tasks for Workflow 1.3."]
        validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
        sub_tasks:
          * id: P1.W3.T1
            description: "**Stage: Authoring Application Code (Completed):** The aggregation script has been pre-authored."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#6.4.3"]
            deliverables: ["Completion of all sub-tasks for Authoring."]
            validation_steps: ["Confirm all sub-tasks are 'done'."]
          * id: P1.W3.T2
            description: "**Stage: Execution & Validation:** Run the aggregation script and validate its outputs."
            status: pending⭕
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.4"]
            deliverables: ["Completion of all sub-tasks for Execution & Validation."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W3.T2.1
                description: "**Execute Aggregation Script:** Run `04_run_comparison.py` to generate the final summary reports."
                status: pending⭕
                depends_on: ["P1.W2.T3.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.6.3"]
                deliverables: ["phases/01_LegacyDB/outputs/reports/comparison_matrix.csv", "phases/01_LegacyDB/outputs/reports/comparison_report.md"]
                validation_steps: ["Verify that 'comparison_matrix.csv' and 'comparison_report.md' are created in the 'outputs/reports' directory."]
          * id: P1.W3.T3
            description: "**Stage: Testing:** Implement and pass all tests for the aggregation workflow."
            status: pending⭕
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-Aggregation-Script", ".windsurf/rules/python_coding_standards.md"]
            deliverables: ["Completion of all sub-tasks for Testing."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W3.T3.1
                description: "**Implement and Pass Tests for Aggregation:** Author and execute a pytest suite for `04_run_comparison.py`, debugging until all tests pass."
                status: pending⭕
                depends_on: ["P1.W3.T2.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-Aggregation-Script"]
                deliverables: ["tests/p1_w3/test_run_comparison.py"]
                validation_steps: ["Run 'pytest tests/p1_w3/test_run_comparison.py' and assert all tests pass."]
      * id: P1.W4
        description: "**Workflow 1.4: Analysis, Reporting, & Recommendation:** Perform the final analysis and synthesize findings into the Phase 1 white paper."
        status: pending⭕
        context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#Workflow-1.4", "phases/01_LegacyDB/README.md"]
        deliverables: ["Completion of all sub-tasks for Workflow 1.4."]
        validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
        sub_tasks:
          * id: P1.W4.T1
            description: "**Stage: Authoring Application Code (Completed):** The notebook templates and initial white paper draft have been pre-authored."
            status: done✅
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#6.4.4"]
            deliverables: ["Completion of all sub-tasks for Authoring."]
            validation_steps: ["Confirm all sub-tasks are 'done'."]
          * id: P1.W4.T2
            description: "**Stage: Execution & Synthesis:** Run the analysis notebooks and produce the final written deliverable."
            status: pending⭕
            context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.5", "phases/01_LegacyDB/PLANNING_PHASE1.md#3.6"]
            deliverables: ["Completion of all sub-tasks for Execution & Synthesis."]
            validation_steps: ["Confirm that the 'status' of all tasks in the 'sub_tasks' list is 'done'."]
            sub_tasks:
              * id: P1.W4.T2.1
                description: "**Execute Individual Analysis Notebooks:** Create and execute 6 copies of the individual analysis notebook to generate reports for each database."
                status: pending⭕
                depends_on: ["P1.W3.T3.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.7.3"]
                deliverables: ["6 executed notebooks in 'phases/01_LegacyDB/notebooks/'."]
                validation_steps: ["Confirm all 6 notebooks execute without errors and all plots render."]
              * id: P1.W4.T2.2
                description: "**Execute Comparative Analysis Notebook:** Create and execute the comparative analysis notebook to generate final comparison visuals."
                status: pending⭕
                depends_on: ["P1.W4.T2.1"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#4.8.3"]
                deliverables: ["1 executed comparative notebook in 'phases/01_LegacyDB/notebooks/'."]
                validation_steps: ["Confirm the notebook executes without errors and the comparative radar plot renders."]
              * id: P1.W4.T2.3
                description: "**Synthesize and Draft Final White Paper:** Integrate all findings from the executed notebooks into the draft white paper to produce the final `Phase1_WhitePaper_v3.md`."
                status: pending⭕
                depends_on: ["P1.W4.T2.2"]
                context_files: ["phases/01_LegacyDB/PLANNING_PHASE1.md#3.6.2", "phases/01_LegacyDB/notebooks/"]
                deliverables: ["phases/01_LegacyDB/drafts/Phase1_WhitePaper_v3.md"]
                validation_steps: ["Confirm the final white paper presents a complete, data-driven argument for the recommended Phase 2 architecture."]

## Phase 2

## Phase 3

## Phase 4

## Phase 5

## Phase 6

## Phase 7

## Phase 8
