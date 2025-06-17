# Windsurf AI Agent - Instructional Memory Architecture

**Version:** 1.0

## Welcome

Welcome to the repository for the Windsurf AI Agent's Instructional Memory system. This project implements a sophisticated, version-controlled architecture designed to guide the behavior and enhance the capabilities of the AI agent, Cascade.

This `README.md` provides a high-level overview of the project, its goals, and how to get started. For a detailed technical breakdown, please refer to the `SYSTEM_ARCHITECTURE.md` file.

## Project Goal

The primary objective of this project is to establish a robust and scalable framework that allows for the precise management of the AI agent's instructional memory. This includes its operational rules, persistent knowledge, and contextual awareness, ensuring that the agent's actions are consistent, predictable, and aligned with user and project requirements.

## Core Architecture

The system is built around a central `.windsurf/` directory, which contains all the necessary components for the agent's operation. This includes:

- **Rules Engine:** A system for defining and applying behavioral rules.
- **Memory Store:** Manages the agent's long-term and short-term memory.
- **Task Management:** A structured system for planning and tracking work.
- **Knowledge Base:** A repository for domain-specific information.

For a complete and detailed explanation of the architecture, please see `SYSTEM_ARCHITECTURE.md`.

---
## Getting Started

To get started with the Instructional Memory system, follow these steps:

1.  **Explore the `.windsurf/` directory:** Familiarize yourself with the structure, especially the `rules/` and `memories/` directories.
2.  **Review the Global Rules:** Read the `rules/global/user_global.rules.md` file to understand the agent's core principles.
3.  **Define Workspace Rules:** If you have project-specific requirements, create a new `.rules.md` file in the `rules/workspace/` directory.
4.  **Start Interacting:** Begin your conversation with the agent. It will automatically load the defined rules and memories.

## How It Works

The system operates in a clear, sequential manner:

1.  **Initialization:** On startup, the agent loads its core configuration and scans for all available rule and memory files.
2.  **Contextual Analysis:** It gathers context from your current workspace, including open files and project settings.
3.  **Rule Application:** The Rule Engine applies the relevant rules based on their activation criteria and priority.
4.  **User Interaction:** The agent processes your prompts, using its instructional memory to guide its responses and actions.

---
## Contributing

Contributions to this project are welcome. If you would like to contribute, please follow the guidelines outlined in the `CONTRIBUTING.md` file. This includes standards for coding, testing, and submitting pull requests.

## License

This project is licensed under the MIT License. See the `LICENSE` file for full details.
