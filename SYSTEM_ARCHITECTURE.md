# System Architecture: Windsurf Instructional Memory

**Version:** 1.0
**Date:** 2025-06-17

## 1.0 Overview

This document outlines the technical architecture for the Windsurf AI Agent's Instructional Memory system. This system is designed to provide a robust, version-controlled framework for managing the agent's behavior, knowledge, and operational context. It is built on principles of modularity, scalability, and clarity to ensure long-term maintainability and extensibility.

## 2.0 Architectural Principles

- **Modularity:** Components are designed as independent, interchangeable modules.
- **Version Control:** The entire architecture, including rules and memories, is designed to be version-controlled via Git.
- **Scalability:** The system is designed to handle a growing set of rules, memories, and contexts without performance degradation.
- **Clarity:** The file structure and content are intended to be human-readable and easily understandable.
- **Context-Awareness:** The system is deeply aware of the user's workspace, project, and current task.

## 3.0 Core Components

The Instructional Memory is composed of several key components that work in concert:

### 3.1 Instructional Memory Core
- **Description:** The central processing unit of the system. It loads rules, memories, and context, and orchestrates the agent's response generation.

### 3.2 Rule Engine
- **Description:** Responsible for parsing and applying behavioral rules defined in `.rules` files. It supports rule activation based on file patterns (globs), manual triggers, or universal application. It also manages rule prioritization.

### 3.3 Memory Store
- **Description:** Manages the agent's persistent and session-based memories. It is divided into three scopes:
    - **Global Memory:** Cross-project rules and settings.
    - **Workspace Memory:** Rules and context specific to the current user workspace.
    - **Session Memory:** Ephemeral data relevant only to the current interaction.

### 3.4 Context Manager
- **Description:** Gathers and provides contextual information to the agent, such as open files, cursor position, and project dependencies.

### 3.5 Knowledge Base
- **Description:** A repository for domain-specific information, technical documentation, and best practices that the agent can draw upon.

### 3.6 Task Management System
- **Description:** A structured system for defining, tracking, and managing development tasks within the `planning/` directory.

---
## 4.0 Directory Structure

The `.windsurf/` directory is the root of the Instructional Memory system. Its structure is organized to separate concerns and provide a clear, logical layout.

```
.windsurf/
├── system.config.json   # Main system configuration
├── rules/               # Rule definitions
│   ├── global/          # Rules applied to all projects
│   ├── workspace/       # Rules for the current workspace
│   └── default.rules.md # Default rule template
├── planning/            # Task management
│   ├── plans/           # Atomic task plans
│   ├── tasks/           # Task tracking
│   └── phase_architecture/ # Project phasing
├── memories/            # Persistent memory
│   ├── global/          # Cross-project memories
│   ├── workspace/       # Workspace-specific
│   ├── imports/         # Auto-imported content
│   └── templates/       # Memory templates
├── context/             # Project context
├── knowledge/           # Domain knowledge
├── hooks/               # Event hooks
├── config/              # Configuration
├── templates/           # File templates
└── tools/               # Custom tools
```

### 4.1 Key Directories Explained

- **`/rules`**: Contains all rule files that govern the agent's behavior. The hierarchy allows for global, workspace, and default rule sets.
- **`/planning`**: Houses all materials related to project and task management, including long-term plans, atomic task definitions, and phase architectures.
- **`/memories`**: Stores persistent data. This includes memories that are global in scope, specific to a workspace, or automatically imported.
- **`/knowledge`**: A repository for structured information and domain-specific knowledge that the agent can reference.
- **`/templates`**: Contains boilerplate templates for various file types, such as new rule files or task definitions, to ensure consistency.

---
## 5.0 File Formats & Templates

To ensure consistency and proper parsing, the system relies on standardized file formats and templates.

### 5.1 Rule File Format

Rule files use a Markdown format with a YAML frontmatter block for configuration.

```markdown
---
activation: AlwaysOn | Glob | Manual
glob: "**/*.ext"  # Required if activation is Glob
priority: 50
description: "Brief description of the rule's purpose."
version: 1.0.0
---
# Rule Title

## Overview
Brief description of what these rules govern.

## Rules
- Rule 1: A specific directive for the agent.
- Rule 2: Another directive.
```

### 5.2 Task File Template

Task files provide a structured way to define work items.

```markdown
## [TASK-ID] [STATUS] [PRIORITY] [EFFORT]

### Description
A clear and concise description of the task.

### Acceptance Criteria
- [ ] Criterion 1 that must be met for the task to be considered complete.
- [ ] Criterion 2.

### Dependencies
- [TASK-ID-Dependency-1]
```

## 6.0 Operational Flow

1.  **Initialization:** The Windsurf agent initializes and loads the `system.config.json`.
2.  **Context Gathering:** The Context Manager collects information about the workspace, project, and user state.
3.  **Rule Loading:** The Rule Engine loads all applicable `.rules.md` files from the `global` and `workspace` directories based on activation criteria.
4.  **Memory Loading:** The Memory Store loads relevant memories from the `global` and `workspace` stores.
5.  **User Prompt:** The agent receives a prompt from the user.
6.  **Processing:** The agent processes the prompt, applying the loaded rules, memories, and context to formulate a response or action plan.
7.  **Execution:** The agent executes the plan, which may involve calling tools, generating code, or modifying files.
8.  **Session Update:** The agent updates the session memory with the results of the interaction.

---

*This architecture provides a comprehensive framework for guiding the Windsurf AI Agent, ensuring its actions are predictable, context-aware, and aligned with project goals.*
