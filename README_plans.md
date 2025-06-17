---
plan_template_version: 1.0.0
last_updated: 2025-06-13
required_context:
  - TASKS.md
  - PLANNING.md
  - docs/architecture.md
  - .windsurf/rules/
---

# Windsurf Plan Files

## 1. Overview

This directory contains all Windsurf Plan files for the Digital TMP project. A "plan" is a detailed, sequential checklist that serves as a high-fidelity script for the Cascade AI agent. Its purpose is to break down a complex task from `TASKS.md` into small, unambiguous, and verifiable atomic actions.

Using these plans ensures that the AI's execution is predictable, reproducible, and precisely aligned with the project's standards and objectives.

## 2. File Naming Convention

All plan files MUST be named according to the ID of the task they correspond to in `TASKS.md`.

**Format:** `<task_id>.plan.md`
**Example:** `p1_w1_t4_1.plan.md`

This convention provides a direct and unambiguous link between a task and its execution script.

## 3. Plan File Structure

Every plan file in this project MUST adhere to the following four-stage structure.

### Preamble

Each plan begins with a metadata block to provide immediate context:

-   `tags`: A list of keywords describing the nature of the task (e.g., `testing`, `coding`, `jupyter`).
-   `task_id`: The specific task ID this plan executes.
-   `description`: A brief, one-sentence summary of the plan's goal.

### Stage 1: Context Ingestion & Verification

This is the most critical setup stage. It is divided into three distinct levels of context that the AI must review before beginning any work.

#### Global Context Review:

This section is **standard and should not change** between plans. It lists the core project-wide documents and the complete rule suite. This ensures every task is executed with full awareness of the project's foundational principles.

**Example:**
```markdown
- [ ] **Global Context Review:** Exhaustively review the following core project files to ensure full alignment with project standards:
    - [ ] `README.md` (root)
    - [ ] `PLANNING.md`
    - [ ] `TASKS.md`
	- [ ] `docs/overview.md`
    - [ ] `docs/architecture.md`
    - [ ] The complete rule suite in the `.windsurf/rules/` directory.
```

#### Phase-Specific Context Review:

This section is **standard for all tasks within a given phase**. It lists documents relevant to the entire phase, such as the phase-specific `README.md` and `PLANNING_PHASE1.md`.

**Example:**
```markdown
- [ ] **Phase-Specific Context Review:** Exhaustively review the following files to understand the present phase and workflow context:
    - [ ] `phases/<current phase>/README.md`
    - [ ] `phases/<current phase>/PLANNING_PHASE<current phase number>.md`
```

#### Task-Specific Context Review:

This section is **unique to each plan** and must be carefully curated. It lists the files, documentation sections (using anchors like `#section-title`), and script outputs that are directly required to execute the specific task at hand.

**Example:**
```markdown
- [ ] **Task-Specific Context Review:** Exhaustively review the following files to understand the specific requirements of task `P1.W1.T4.1`:
    - [ ] The test strategy section of the Phase 1 plan: `phases/01_LegacyDB/PLANNING_PHASE1.md#Testing-Strategy-for-00_setup_databases.py`.
    - [ ] The Python script that is the subject of the tests: `phases/01_LegacyDB/src/00_setup_databases.py`.
```

### Stage 2: Preparation

This stage involves setting up all necessary prerequisites for the core task. Actions in this stage typically include:

-   Defining lists of items to iterate over (e.g., database names, file paths).
-   Identifying source template files and target deliverable paths.
-   Confirming that inputs from previous tasks exist and are accessible.

### Stage 3: Execution

This is the primary "work" stage of the plan. It contains the detailed, step-by-step instructions for the AI to follow. These steps should be:

-   **Atomic:** Each step should represent a single, clear action.
-   **Sequential:** The order of the steps must reflect the logical flow of the work.
-   **Imperative:** Each step should be a clear command (e.g., "Create the file...", "Execute the script...", "Verify the output...").

### Stage 4: Final Validation & Cleanup

This final stage ensures the task was completed successfully and integrates the results back into the project's tracking system.

-   Verify that all deliverables specified in `TASKS.md` have been created correctly.
-   Explicitly check all `validation_steps` from `TASKS.md`.
-   Propose the necessary changes to `TASKS.md` to update the task's status to `done`.

---
## 4. Authoring New Plan Files: A Step-by-Step Guide

Follow this process to create a new plan file for any task.

### Step 1: Identify the Target Task
Locate the `pending` leaf-node task in `TASKS.md` that you need to accomplish. Read its `id`, `description`, `context_files`, `deliverables`, and `validation_steps` carefully.

### Step 2: Create and Name the Plan File
Create a new file in this directory (`.windsurf/plans/`). Name it using the exact `id` of the task. For example, for task `P1.W5.T1.1`, the file will be `p1_w5_t1_1.plan.md`.

### Step 3: Draft the Preamble
Start the file with the metadata block. Fill in the `task_id` and write a `description` that summarizes the task's goal. Choose relevant `tags`.

### Step 4: Populate the Context Stages
Use an existing plan file as a template for this section.
1.  **Global Context:** Copy and paste this section verbatim. **Do not modify it.**
2.  **Phase-Specific Context:** Copy this section from another plan within the same phase.
3.  **Task-Specific Context:** This is the only context section you must edit. List all the files and document sections specified in the `context_files` array for your target task in `TASKS.md`.

### Step 5: Atomize the Execution Steps
Read the `description` and `deliverables` for the task. Break down the work into the smallest possible, sequential actions and list them as checklist items under `## Stage 3: Execution`.
-   **Good (Atomic):** `Create the empty file 'report.md'`.
-   **Bad (Not Atomic):** `Write the report`.
-   **Good (Atomic):** `Add the title 'Final Report' to 'report.md'`. `Add a section header '## Introduction'`.

### Step 6: Define Validation
Under `## Stage 4: Final Validation & Cleanup`, create checklist items that directly correspond to the `validation_steps` listed for the task in `TASKS.md`. Add a final step to propose the status update to the `TASKS.md` file.

### Step 7: Review
Read through your completed plan. Does it flow logically? Is every step a clear, unambiguous command? Does it produce all the required deliverables?

---
## 5. LLM-Specific Instructions

### For AI Assistants (like Cascade)

When generating or updating plan files, follow these guidelines:

1. **Initial Analysis**
   - First analyze `TASKS.md` for the specific task requirements
   - Review all context files listed in the task's `context_files`
   - Understand the task's dependencies and validation steps

2. **Plan Generation**
   - Always follow the 4-stage structure (Context, Preparation, Execution, Validation)
   - Use the exact file paths and naming conventions from the project
   - Include all validation steps from `TASKS.md`
   - Make steps atomic and verifiable

3. **Validation Script**
   ```python
   # Example validation for plan files
   import os
   import yaml
   import re
   from pathlib import Path

   def validate_plan(plan_path):
       """Validate a plan file against project standards."""
       required_sections = [
           "Context Ingestion & Verification",
           "Preparation",
           "Execution",
           "Final Validation & Cleanup"
       ]

       # Check file exists and is readable
       if not os.path.exists(plan_path):
           return False, f"Plan file not found: {plan_path}"

       # Check file naming convention
       if not re.match(r'p\d+_w\d+_t\d+_\d+\.plan\.md$', os.path.basename(plan_path).lower()):
           return False, "Invalid filename format. Expected format: p#_w#_t#_#.plan.md"

       # Check required sections exist
       content = Path(plan_path).read_text(encoding='utf-8')
       missing_sections = [s for s in required_sections if f"## {s}" not in content]
       if missing_sections:
           return False, f"Missing required sections: {', '.join(missing_sections)}"

       return True, "Plan validation passed"
   ```

4. **Generation Prompt Template**
   ```
   Create a new plan for task [TASK_ID] following these steps:
   1. Analyze the task in TASKS.md and its context files
   2. Create a markdown file named [TASK_ID].plan.md
   3. Include all four standard sections
   4. Make steps atomic and verifiable
   5. Include all validation criteria from TASKS.md
   ```

## 6. Example: Creating a Plan for a Hypothetical Task

Let's assume the following task exists in `TASKS.md`:

**Hypothetical Task in `TASKS.md`:**
```yaml
- id: P1.W5.T1.1
  description: "**Generate Data Flow Diagram:** Create a PNG diagram summarizing the data flow from legacy to benchmark databases."
  status: pending⭕
  depends_on: ["P1.W4.T2.3"]
  context_files: ["docs/architecture.md#data-flow", "phases/01_LegacyDB/outputs/reports/comparison_report.md"]
  deliverables: ["phases/01_LegacyDB/outputs/diagrams/data_flow_summary.png"]
  validation_steps: ["Confirm 'data_flow_summary.png' exists in the 'outputs/diagrams' directory and is not a zero-byte file."]
```

**Resulting Plan File (`p1_w5_t1_1.plan.md`):**
```markdown
# Plan: Generate Data Flow Diagram
tags: [documentation, diagram, reporting]
task_id: P1.W5.T1.1
description: A plan to generate a PNG diagram summarizing the project's data flow.

## Stage 1: Context Ingestion & Verification
- [ ] **Global Context Review:** (This section would be copied verbatim from another plan)
- [ ] **Phase-Specific Context Review:** (This section would be copied for Phase 1)
- [ ] **Task-Specific Context Review:** Exhaustively review the following:
    - [ ] `docs/architecture.md#data-flow`
    - [ ] `phases/01_LegacyDB/outputs/reports/comparison_report.md`

## Stage 2: Preparation
- [ ] Identify the target deliverable path: `phases/01_LegacyDB/outputs/diagrams/data_flow_summary.png`.
- [ ] Review the information in the context files to synthesize the key entities and data movement steps to be included in the diagram.

## Stage 3: Execution
- [ ] Create a new diagramming script or use a tool to define the following nodes: "Legacy DBs (DF8, DF9, etc.)", "Staging Area", "Benchmark DBs (Wide Numeric, etc.)".
- [ ] Add directed arrows between the nodes to represent the data flow as described in the context.
- [ ] Export the final diagram to the target deliverable path: `phases/01_LegacyDB/outputs/diagrams/data_flow_summary.png`.

## Stage 4: Final Validation & Cleanup
- [ ] Verify that the file `phases/01_LegacyDB/outputs/diagrams/data_flow_summary.png` has been created.
- [ ] Confirm that the file size of `data_flow_summary.png` is greater than zero bytes.
- [ ] Propose the required changes to `TASKS.md` to update the status of task `P1.W5.T1.1` to `done`.
```
