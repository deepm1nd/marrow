# Planning Phase Guide
v.0.0.03

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. Best Practices for Planning](#3-best-practices-for-planning)
- [4. Task Management for Planning](#4-task-management-for-planning)
  - [4.1. Development Plan Checklist Protocol](#41-development-plan-checklist-protocol)
- [5. Recommended Outline for a Development Plan](#5-recommended-outline-for-a-development-plan)
  - [5.1. Introduction](#51-introduction)
  - [5.2. Technology Stack](#52-technology-stack)
  - [5.3. Project Folder Structure](#53-project-folder-structure)
  - [5.4. Phases and Milestones](#54-phases-and-milestones)
  - [5.5. Task Decomposition](#55-task-decomposition)
  - [5.6. Development Checklist Generation](#56-development-checklist-generation)
  - [5.7. Development Prompt Generation](#57-development-prompt-generation)
  - [5.8. Test Strategy & Plan](#58-test-strategy--plan)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Planning Phase, where an `Architecture Specification` is translated into an actionable roadmap for development.

## 2. Goal
The goal of this phase is to produce three key artifacts:
1.  A comprehensive **Development Plan**.
2.  A corresponding **Development Checklist**.
3.  A **Development Prompt File** for the development agent.

## 3. Best Practices for Planning
- **Decomposition:** Break down the work into small, independent, and actionable tasks. This allows for parallel development and reduces the risk of merge conflicts.
- **AI Agent Utilization:** Phase and task decomposition should target no more than 40% utilization of a state-of-the-art AI Agent (like Google's Jules) as measured by current capacity metrics (e.g., token count). This ensures the agent has sufficient capacity for robust reasoning, self-correction, and handling unforeseen complexities.
- **Traceability:** Ensure that every task in the development plan can be traced back to a specific requirement or component in the `Architecture Specification`.
- **Clarity:** Each task should have a clear description of the work to be done, the expected outcome, and any dependencies.
- **Checklist-Driven:** The development plan must be accompanied by a checklist. This checklist will be used to track the progress of development tasks in a granular way.

## 4. Task Management for Planning
### 4.1. Development Plan Checklist Protocol
- With every development plan, the agent must generate a checklist file (`[projectname]_checklist.md`) that matches the Development Plan.
- The agent is responsible for updating the checklist with the status of each task.

## 5. Recommended Outline for a Development Plan
The following outline should be used as the structure for any `*_development_plan.md` document created during the Planning Phase.

### 5.1. Introduction
- **Purpose:** State the purpose of the document.
- **Scope:** Briefly describe the features or components covered by this plan.
- **References:** Link to the `Architecture Specification` document.

### 5.2. Technology Stack
- List the primary languages, frameworks, and technologies that will be used for the project, as defined in the Architecture Specification.

### 5.3. Project Folder Structure
- Provide a tree-view of the proposed folder structure for the project, ensuring it aligns with the conventions in `AGENTS.MD` (`src/` for code, `docs/` for documentation, etc.).

### 5.4. Phases and Milestones
- Break down the development work into logical phases (e.g., Phase 1: User Authentication, Phase 2: Order Management).
- Define clear milestones and deliverables for each phase.

### 5.5. Task Decomposition
This is the core of the development plan. For each phase, provide a detailed list of tasks.

- **Task ID:** A unique identifier (e.g., `TASK-001`).
- **Description:** A clear and concise description of the task.
- **Component(s):** The component(s) from the Architecture Specification that this task relates to.
- **Dependencies:** Any other tasks that must be completed before this one can start.
- **Estimated Effort:** For an AI Agent, this should include % of context window capacity and the actual metric value (e.g., token count). For human tasks, estimate in hours.
- **Exit Criteria:** A clear definition of what "done" means for this task.
- **Assignee (Optional):** The team member or agent responsible for the task.

**Example Task:**
- **Task ID:** `AUTH-001`
- **Description:** Create the database migration script for the `users` table, including fields for `id`, `username`, `email`, and `password_hash`.
- **Component(s):** `Database`
- **Dependencies:** None
- **Estimated Effort:** 15% capacity (e.g., 12k tokens).
- **Exit Criteria:** A migration script is created, and it runs successfully against a local database instance.
- **Testing Requirements:** N/A for database migrations. A subsequent task will seed the DB and test authentication.

### 5.6. Development Checklist Generation
- This section should state that a corresponding checklist (`[projectname]_checklist.md`) will be generated from this development plan.
- The checklist should list every Task ID from the Task Decomposition section, with a status indicator (e.g., To Do, In Progress, Done).

### 5.7. Development Prompt Generation
- This section should specify that a development prompt file (e.g., `p_[projectname]_dev.md`) will be generated.
- This prompt is intended for a new Jules instance (or a sequence of them) and must contain the following instructions for the agent's workflow:
  1.  **Check the Checklist:** The agent must first consult the `[project_name]_checklist.md` file to identify the next unfinished task.
  2.  **Confirm Code Base:** The agent must verify that the current state of the code base matches the expectations of the checklist.
  3.  **Execute Task:** The agent is to complete the single, unfinished task.
  4.  **Build and Test:** After implementing the changes, the agent must build the project and run all relevant tests to ensure the changes are correct and have not introduced any regressions.
  5.  **Update Checklist:** Upon successful completion and verification of the task, the agent must update the `[project_name]_checklist.md` file to mark the task as complete.
  6.  **Update Handoff Notes:** The agent must update the `handoff_notes.md` and `open_issues.md` files with a summary of the changes, any new issues that have arisen, and any other relevant information for handoff.
  7.  **Submit Changes:** The agent must submit all changes with a unique, short, and descriptive branch name. The commit message must be clear and detailed, incorporating all comments from the `handoff_notes.md` and `open_issues.md` files.

### 5.8. Test Strategy & Plan
- This section is mandatory and must detail the strategy for all three levels of testing.
- **Unit Test Plan:** Identify the critical functions and modules that require unit tests.
- **Integration Test Plan:** Describe how major components will be tested together, including which interfaces need mocking or contract tests.
- **System & Acceptance Test Plan:** Translate the Acceptance Criteria from the Design phase into a concrete set of test cases. This includes specifying the required test data, environment setup, and whether the test will be automated or performed manually.

### 5.9. Logging Strategy
- The development plan must incorporate the logging strategy defined in the `Architecture Specification`.
- The plan must include tasks for instrumenting the code with appropriate log messages at all 8 levels:
    - **Emergency:** The system is unusable.
    - **Alert:** Action must be taken immediately.
    - **Critical:** Critical conditions.
    - **Error:** Error conditions in the system.
    - **Warning:** A warning condition.
    - **Notice:** A normal but significant condition. (Default Level)
    - **Informational:** Informational messages.
    - **Debug:** Debug-level messages, used for debugging.
- The plan should account for making the log level configurable at launch.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-07-30 | Jules       | Initial creation of the guide.        |
| 0.0.02  | 2025-07-30 | Jules       | Added Exit Criteria and AI utilization guidelines to Planning Phase. |
| 0.0.03  | 2025-07-30 | Jules       | Added Technology Stack and Folder Structure to recommended outline. |
