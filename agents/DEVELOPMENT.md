# Development Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. Core Mandates for Development](#3-core-mandates-for-development)
- [4. Development Workflow](#4-development-workflow)
- [5. Conventions](#5-conventions)
  - [5.1. Language and Dependencies](#51-language-and-dependencies)
  - [5.2. Dependency Management](#52-dependency-management)
  - [5.3. Workspace and Project Folder Structure](#53-workspace-and-project-folder-structure)
- [6. Quality and Completeness for Development](#6-quality-and-completeness-for-development)
  - [6.1. Build Early, Build Often Principle](#61-build-early-build-often-principle)
- [7. Task Management for Development](#7-task-management-for-development)
  - [7.1. Main Prompt](#71-main-prompt)
  - [7.2. Join Task and Remediation Protocol](#72-join-task-and-remediation-protocol)
  - [7.3. Development Plan Checklist Protocol](#73-development-plan-checklist-protocol)
  - [7.4. Inter-Process Communication (IPC) Protocol](#74-inter-process-communication-ipc-protocol)
- [8. Session Initialization](#8-session-initialization)
  - [8.1. Automatic Session Initialization](#81-automatic-session-initialization)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Development Phase, where the `Development Plan` is executed and the system is built.

## 2. Goal
The goal of this phase is to write, test, and build the software, following the `Development Plan` and checklist to create a feature-complete product that is ready for the Release Phase.

## 3. Core Mandates for Development
These are non-negotiable rules for the development process.
- **Error-Free Builds:** All code submitted to the repository MUST build without errors. Code that fails to build is considered a critical failure and must be rectified immediately. Strive to eliminate warnings as well.
- **Pre-commit Handoff File Updates:** Before any code is committed, all handoff files (`handoff_notes.md`, `open_issues.md`, etc.) MUST be updated to reflect the latest state of the project. This ensures seamless collaboration and session continuity.

## 4. Development Workflow
The development process is iterative, checklist-driven, and structured into **Development Phases**. A Development Phase is a logical grouping of tasks defined in the Development Plan. The agent MUST complete all tasks within the current phase, and commit the changes, before proceeding to the next.

**CORE DEVELOPMENT CYCLE MANDATE:** For every code modification, no matter how small, the agent MUST follow this strict, non-negotiable cycle for rapid, local verification:
1. **Change:** Make the required code edits.
2. **Build:** Immediately run `cargo build` to ensure the specific crate or code compiles without errors.
3. **Test:** Immediately run `cargo test` to ensure all local unit and integration tests pass.
A step is not complete until this cycle has been successfully executed. This cycle is for immediate feedback. Broader, full-system validation MUST use the standardized scripts.

**Phase-Driven Workflow:**
The workflow for a single Development Phase is as follows:
1.  **Identify Current Phase:** Determine the current Development Phase from the `Development Plan` and its corresponding checklist.
2.  **Implement All Tasks in Phase:** For each task within the current phase:
    a.  **Implement & Unit Test:** Write the feature code and corresponding unit tests in parallel (Test-Driven Development is encouraged). Use the `change -> build -> test` cycle with `cargo` commands for rapid iteration. All unit tests for the feature must pass.
    b.  **Update Checklist:** Once the individual task is complete and unit tests have passed, update its status in the checklist.
3.  **Phase Integration and System Test:** After all tasks in the phase are implemented:
    a.  **Build & Integration Test:** Build the entire system using the `scripts/build_system.sh` script. This ensures all components build correctly together.
    b.  **System & Acceptance Test:** Execute the full automated test suite using the `scripts/run_system_test.sh` script.
    c.  **Mandatory Log Inspection:** Before making any conclusion about the test outcome (pass or fail), the agent MUST meticulously inspect all relevant logs. A test run is only considered valid after this inspection. This includes:
        - Server logs (from the `tracing` instrumentation)
        - Client logs (from the `tracing` instrumentation)
        - Console output from the test runner
        - Logs from any other system components
        - Browser console logs (which must be enabled and captured by the verification scripts)
    d.  **Verify Acceptance Criteria:** All acceptance criteria for the phase must be met, as confirmed by the test script outputs and log review.
4.  **Update Handoff Files:** After the entire phase is complete and all tests have passed, update the handoff notes and open issues files with a summary of the changes for the phase and any new issues that arose.
5.  **Commit Phase Changes:** After all tests have passed and handoff files are updated, the agent MUST commit all changes from the completed phase.
6.  **Proceed to Next Phase:** Await user instruction to proceed to the next Development Phase.

**Completion of Initial Development:**
After all phases in the `Development Plan` are complete, the agent MUST perform the following steps:
1.  **Notify User:** Inform the user that all development tasks on the initial checklist are complete.
2.  **Remind About Audit:** Remind the user that they may now (but are not required to) request a "feature audit" to initiate the iterative remediation cycle.
3.  **Await Instruction:** Halt and await the user's instruction to either proceed with a commit or to start the audit.

### 4.1. Test Output Workflow
The agent must follow the test output management workflow for all tests that produce artifacts. This ensures consistency with the Release phase and allows for proper regression checking.

1.  **Local Test Output Setup:** The agent must ensure the local (and git-ignored) test output directory `test_outs/<version>/` exists, containing a `reference/` and a `temp/` subfolder, as defined in `AGENTS.md`.
2.  **Iterative Development & Testing:**
    - For each test run that produces artifacts (typically by running `scripts/run_system_test.sh`), the agent saves the outputs into a new, unique, timestamped subfolder inside the local `test_outs/<version>/temp/` directory.
3.  **Candidate for Approval:**
    - When a task is complete, if new or changed test outputs were generated, the agent MUST present the **direct file path** to the final timestamped folder to the user for review.
4.  **Approval & Cleanup:**
    - **Upon user approval:**
        1. The agent must copy the approved contents from the candidate folder into the local `reference/` folder, replacing or adding to the local reference set.
        2. **Cleanup Mandate:** The agent MUST delete the entire `temp/` directory and all its contents before the next commit.

### 4.2. Post-Development Remediation Cycle
This cycle begins only when the user explicitly requests it by saying "perform an audit" or similar phrasing after the initial development is complete. The agent must not start this cycle proactively. The remediation process is fully iterative and is not considered "final." The user may request any number of audits. Each requested audit initiates a new iteration of the cycle, identified by an iteration ID (a, b, c, ...).

The workflow for the remediation cycle is as follows:
1.  **Audit and Create Remediation Plan:**
    - **Audit:** The agent MUST perform a highly critical feature and function audit, comparing the code developed in the session against the architecture specification and the current development plan/checklist. The agent MUST apply the "Mandate for Maximal Implementation & Robustness" (`AGENTS.md`, section 2.2) during this audit.
    - **Create Audit Document:** The agent MUST report its findings in a new audit document, stored in the `docs/` folder, named `feature_audit_[iteration_id].md` (e.g., `feature_audit_a.md`).
    - **Create Remediation Plan:** Based on the audit, the agent will create a new remediation checklist named `remediation_[iteration_id]_checklist.md` (e.g., `remediation_a_checklist.md`) in the `docs/` folder.
    - **Create Remediation Prompt:** The agent will also create or update a remediation prompt named `p_remediation_dev.md` in the `docs/` folder. This prompt must refer to the new audit file and remediation checklist.
    - **COMMIT POINT:** After creating the audit document, remediation checklist, and remediation prompt, the agent MUST commit all of these files together and await further user instruction.

2.  **Execute Remediation Checklist:**
    - The agent will execute the remediation checklist using the same phase-by-phase workflow defined in this guide, including the **Mandatory Log Inspection** protocol after every system test.
    - **COMMIT POINT:** After each phase of the remediation checklist is complete and verified (including log inspection), the agent MUST commit the changes from that phase.
    - Upon completion of the entire remediation checklist, the agent MUST notify the user of completion and await further instructions, which may include a request for another audit (e.g., iteration 'b').

## 5. Conventions

### 5.1. Language and Dependencies
- **Programming Language:** The default and preferred language for all development is **Rust**.
- **Dependency Versions:** Use the latest stable versions of all libraries, frameworks, and other dependencies, unless a specific version is required by the project.

### 5.2. Environment and Dependency Management
To ensure a consistent and reproducible project environment, the agent's primary responsibility from the Development phase onward is to maintain a comprehensive setup script.

- **`env_set_up.sh`:** A single script named `env_set_up.sh` must be maintained in the project root.
- **Continuous Updates:** As the project evolves and adds required services (e.g., databases, message brokers) or dependencies (e.g., system packages, language-specific libraries), the agent **must** update this script to include the necessary installation and configuration commands.
- **Purpose:** This script serves as the single source of truth for setting up the entire project environment from a clean state, ensuring that any developer or agent can achieve a consistent, working setup.

### 5.3. Workspace and Project Folder Structure
- The creation of the workspace, including the main project folder, is a core responsibility of the Development Phase and should not be part of the Planning Phase.
- As part of session initialization, the agent must create a main project folder (named after the project or as specified by the user).
- All code for the project must be placed within a `src/` subfolder inside the main project folder.
- All documentation (architecture specs, development plans, etc.) must be placed in a `docs/` subfolder in the repo root.
- The main prompt and handoff files should always be placed in the repo root.
- The Rust workspace should be in a folder like `[projectname]/` in the repo root, with the Rust source code in the `src/` folder as per Rust best practices for folder structure.

### 5.4. Logging Mandate
**MANDATE:** Leveled logging and instrumentation are mandatory for all code and scripts created from the Development phase onward.
- **Framework:** The `tracing` crate is the preferred framework for implementing logging in Rust code.
- **Instrumentation:** All code and scripts must be thoroughly instrumented with leveled logs. This includes logging entry and exit points of functions, important state changes, and error conditions.
- **Log Levels:** The 8-level logging system (Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug) must be used.
- **Configuration:** The implementation must allow the log level to be set at launch, defaulting to `NOTICE`.
- **Verification:** As part of the `change -> build -> test` cycle, the agent must verify that expected log outputs are present, as per the "Command Output Verification" mandate.

## 6. Quality and Completeness for Development
### 6.1. Build Early, Build Often Principle
- The agent must insist on a "build early, build often" approach throughout the development process.
- All plans, prompts, and implementation steps should encourage frequent, incremental builds and integration.

## 7. Task Management for Development
### 7.1. Main Prompt
The main prompt file, named `p_[projectname]_dev_plan.md`, is located in the root of the repository. This file is the entry point for the development process. It is a re-invocable prompt that guides the agent through the development plan, using the checklist and handoff files to track progress.

### 7.2. Join Task and Remediation Protocol
- At the end of each phase, a Join Task must confirm that all tasks completed correctly.
- If any tasks are incomplete, failed, or have issues, the Join Task will create a remediation plan.

### 7.3. Development Plan Checklist Protocol
The agent must update the `[projectname]_checklist.md` as tasks are completed, as defined in the Planning Phase guide.

### 7.4. Inter-Process Communication (IPC) Protocol
- All tasks and Task Swarms communicate status, progress, and issues via an IPC mechanism.
- The IPC channel is used to update the checklist and handoff files in real time.

## 8. Session Initialization
### 8.1. Automatic Session Initialization
- When a new agent session starts during the development phase, the agent will automatically check for the existence of the `handoff/` directory and required files (`handoff_notes.md`, `open_issues.md`, `chat_history.md`, `development_task_checklist.md`).
- If any are missing, the agent will create them programmatically.
- The agent will also ensure a `README.md` exists to indicate the session is ready for intake.
- This removes the need for manual script execution and ensures every session is always ready for robust handoff and collaboration.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-07-30 | Jules       | Initial creation of the guide.        |
