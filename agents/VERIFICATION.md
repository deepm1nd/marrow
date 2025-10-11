# Verification Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. The Verification Lifecycle](#3-the-verification-lifecycle)
  - [3.1. Stage 1: Verification Planning](#31-stage-1-verification-planning)
  - [3.2. Stage 2: Verification Script Development](#32-stage-2-verification-script-development)
  - [3.3. Stage 3: Test Execution](#33-stage-3-test-execution)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Verification Phase, a critical step that follows the Development Phase. This phase ensures that the implemented software meets the precise acceptance criteria laid out in the `Architecture Specification`.

## 2. Goal
The goal of this phase is to systematically verify every functional and non-functional requirement through automated tests and user-approved evidence. Successful completion of this phase provides high confidence that the system is ready for the Release Phase.

## 3. The Verification Lifecycle
The Verification Phase is divided into three distinct stages.

### 3.1. Stage 1: Verification Planning
The first stage is to create a comprehensive plan for the verification process.
1.  **Create Verification Plan:** The agent must create a `verification_plan.md` document. This plan will outline the scope, strategy, and schedule for verifying the requirements.
2.  **Create Verification Checklist:** The agent must create a `verification_checklist.md` document. This checklist will list every Requirement ID from the `Architecture Specification` and will be used to track the verification status of each item.
3.  **Create Verification Prompt:** The agent must create a `p_verification.md` prompt. This prompt will serve as the entry point for the verification work and will reference the `Architecture Specification`, `verification_plan.md`, and `verification_checklist.md`.

### 3.2. Stage 2: Verification Script Development
This stage focuses on creating the automated scripts necessary to execute the verification tests.
1.  **Create Test Runner:** The agent MUST create a primary test runner script at `scripts/verification/run_verification_test.sh`.
    -   **Parameterized Execution:** This script MUST be designed to accept a Requirement ID (e.g., `PROJ-FUNC-REQ-0001`) as a command-line parameter.
    -   **Orchestration:** The script will use this parameter to call the specific verification script corresponding to that requirement.
2.  **Develop Individual Verification Scripts:** For each requirement, the agent will develop a dedicated verification script.
    -   These scripts will use appropriate tools (e.g., Playwright for UI tests, custom scripts for API tests) to execute the test and capture evidence.
    -   Evidence can be a screenshot, a log file, or other console output that proves the acceptance criterion has been met.

### 3.3. Stage 3: Test Execution
This stage is an iterative cycle of testing, verifying results, and fixing any issues that arise.
1.  **Execute a Test:** The agent will run the test runner for a specific Requirement ID: `scripts/verification/run_verification_test.sh [REQUIREMENT_ID]`.
2.  **Present Evidence to User:**
    -   **MANDATE: The agent is explicitly forbidden from marking any test, acceptance criterion, or requirement as complete.** This decision rests solely with the user.
    -   For visual evidence, the agent MUST use the `frontend_verification_complete(screenshot_path: "path/to/screenshot.png")` tool to present any captured screenshots.
    -   For textual evidence (e.g., logs, API responses), the agent MUST use the `message_user(message: str, continue_working: bool)` tool. The agent should provide the minimal, most relevant text required for the user to make a determination.
3.  **Await User Approval:** The agent must stop and wait for the user to explicitly approve the evidence and mark the acceptance criterion and its parent requirement as complete.
4.  **Iterate or Fix:**
    -   If the user approves the test, the agent updates the `verification_checklist.md` and proceeds to the next requirement.
    -   If the user rejects the test, the agent must enter a `test -> verify -> fix` cycle, making necessary code changes and re-running the test until the user approves the evidence.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-10-10 | Jules       | Initial creation of the guide.        |