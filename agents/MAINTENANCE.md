# Maintenance Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. The Maintenance Lifecycle: A Mini-Project Workflow](#3-the-maintenance-lifecycle-a-mini-project-workflow)
  - [3.1. Stage 1: Feature Intake](#31-stage-1-feature-intake)
  - [3.2. Stage 2: Architecture Update (Design)](#32-stage-2-architecture-update-design)
  - [3.3. Stage 3: Scoped Planning](#33-stage-3-scoped-planning)
  - [3.4. Stage 4: Scoped Development](#34-stage-4-scoped-development)
  - [3.5. Stage 5: Scoped Verification](#35-stage-5-scoped-verification)
  - [3.6. Stage 6: Incremental Release](#36-stage-6-incremental-release)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Maintenance Phase, which is not a passive monitoring period but an active, cyclical process for implementing new features and bug fixes. All session-level rules are defined in `AGENTS.md` and all script and command rules are in `agents/SCRIPT_RULES.md`. Both MUST be adhered to at all times.

## 2. Goal
The goal of this phase is to provide a structured, iterative framework for the ongoing evolution of the software. It ensures that new features and fixes are integrated into the existing architecture and released in a controlled manner.

## 3. The Maintenance Lifecycle: A Mini-Project Workflow
The Maintenance Phase is a microcosm of the entire development lifecycle, applied to a smaller scope.

### 3.1. Stage 1: Feature Intake
The cycle begins when the user provides a description or list of new features or bug fixes to be implemented (e.g., by referencing a feature proposal document).

### 3.2. Stage 2: Architecture Update (Design)
The agent analyzes the requested features and proposes additive changes to the existing `Architecture Specification`.
-   **Process:** This includes adding new functional and non-functional requirements with unique IDs, and defining their corresponding acceptance criteria.
-   **Approval:** This is an iterative process that requires explicit user approval for the proposed architectural changes before proceeding.

### 3.3. Stage 3: Scoped Planning
Once the architecture changes are approved, the agent creates a new, scoped `Development Plan`, `Checklist`, and `Prompt` specifically for the new features.
-   **Process:** The agent will follow the workflow defined in the `agents/DESIGN.md` guide, but scoped only to the newly approved requirements.
-   **Approval:** This stage is also iterative and requires explicit user approval.

### 3.4. Stage 4: Scoped Development
The agent executes the scoped development plan to implement the new features.
-   **Process:** The agent will follow the workflow defined in `agents/DEVELOPMENT.md`.

### 3.5. Stage 5: Scoped Verification
The newly developed features are verified against their corresponding acceptance criteria.
-   **Process:** The agent will follow the workflow defined in `agents/VERIFICATION.md`, using the parameterized test runner to execute tests for the new requirement IDs.

### 3.6. Stage 6: Incremental Release
The project, with its new features, is released with an incremented version number.
-   **Process:** The agent will follow the workflow defined in `agents/RELEASE.md`.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-10-11 | Jules       | Initial creation of the guide.        |