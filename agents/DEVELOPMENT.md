# Development Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. Session Prompt](#3-session-prompt)
- [4. Phase-Specific Mandates](#4-phase-specific-mandates)
- [5. Development Workflow](#5-development-workflow)
  - [5.1. Core Development Cycle](#51-core-development-cycle)
  - [5.2. Phase-Driven Workflow](#52-phase-driven-workflow)
  - [5.3. Completion of Initial Development](#53-completion-of-initial-development)
  - [5.4. Post-Development Remediation Cycle](#54-post-development-remediation-cycle)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Development Phase. All session-level rules are defined in `AGENTS.md` and all script and command rules are in `agents/SCRIPT_RULES.md`. Both MUST be adhered to at all times.

## 2. Goal
The goal of this phase is to write, test, and build the software, following the `Development Plan` and checklist to create a feature-complete product that is ready for the Verification Phase.

## 3. Session Prompt
**MANDATE:** At the beginning of this phase, a re-invocable prompt file (e.g., `p_development.md`) must be created. This prompt will guide the agent through the development plan, using the checklist and handoff files to track progress.

## 4. Phase-Specific Mandates
- **Error-Free Builds:** All code submitted to the repository MUST build without errors. Code that fails to build is considered a critical failure and must be rectified immediately. Strive to eliminate warnings as well.

## 5. Development Workflow
The development process is iterative and checklist-driven.

### 5.1. Core Development Cycle
For every individual task, the agent MUST follow the iterative build-fix loop defined in `agents/SCRIPT_RULES.md`.

**Exception for Batched Tasks:** If several tasks are tightly interrelated and would be more efficient to implement at once, the agent MUST request permission from the user to batch these tasks into a single build cycle.

### 5.2. Phase-Driven Workflow
The workflow for a single Development Phase is as follows:
1.  **Identify Current Phase:** Determine the current Development Phase from the `Development Plan` and its corresponding checklist.
2.  **Implement All Tasks in Phase:** For each task within the current phase, the agent must follow the **Core Development Cycle**. Once the task is implemented and builds successfully, the agent will update the checklist.
3.  **Phase Integration and System Test:** After all tasks in the phase are implemented, build the system using `scripts/build_system.sh` and test it using `scripts/run_system_test.sh`. The agent must perform a mandatory log inspection before concluding the test outcome.
4.  **Pre-Commit Verification:** Before committing, the agent MUST verify that all documentation is up-to-date in accordance with the **Mandate for Pre-Commit Documentation Integrity**.
5.  **Commit Phase Changes:** After all tests have passed and all documentation has been verified, the agent MUST commit all changes from the completed phase.
6.  **Proceed to Next Phase:** Await user instruction to proceed to the next Development Phase.

### 5.3. Completion of Initial Development
After all phases in the `Development Plan` are complete, the agent must notify the user and await instruction to proceed to the **Verification Phase** or to start a post-development remediation cycle.

### 5.4. Post-Development Remediation Cycle
This cycle begins only when the user explicitly requests it by saying "perform an audit".
1.  **Audit and Create Remediation Plan:** The agent will perform a feature audit and create the necessary planning documents.
    -   **COMMIT POINT:** After creating all documents, the agent MUST commit them together and await further user instruction.
2.  **Execute Remediation Checklist:** The agent will execute the remediation checklist using the phase-by-phase workflow.
    -   **COMMIT POINT:** After each phase of the remediation checklist is complete and verified, the agent MUST commit the changes.
    -   Upon completion, the agent MUST notify the user and await further instructions.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-10-10 | Jules       | Initial creation and refactoring.     |