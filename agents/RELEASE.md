# Release Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. The Goal of the Release Phase](#2-the-goal-of-the-release-phase)
- [3. Best Practices for Release](#3-best-practices-for-release)
- [4. The Release Lifecycle](#4-the-release-lifecycle)
  - [4.1. Release Planning](#41-release-planning)
  - [4.2. Preparation Stage](#42-preparation-stage)
  - [4.3. Test Stage](#43-test-stage)
  - [4.4. Deployment Stage](#44-deployment-stage)
  - [4.5. Post-Release Monitoring](#45-post-release-monitoring)
- [5. Quality and Completeness for Release](#5-quality-and-completeness-for-release)
  - [5.1. Go/No-Go Decision](#51-gono-go-decision)
  - [5.2. Documentation Mandate](#52-documentation-mandate)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Release Phase, the bridge between a tested product and its public launch. It covers final packaging, documentation, and communication.

## 2. Goal
The goal of this phase is to execute a smooth, controlled, and successful product launch. This includes verifying the release candidate's stability, publishing all documentation, and effectively communicating new features to the target audience.

### 2.1. Agent Workflow
**MANDATE:** When entering the Release Phase, the agent must work through this guide sequentially. It must start at section `4.1. Release Planning`, present the plan to the user for approval, and then proceed progressively through sections 4.2 to 4.5, providing updates and seeking approval as needed at each major stage.

**Process Deviations:**
- If the agent finds it necessary to deviate from the established release plan, it must first get approval from the user to update the plan itself. The plan should be treated as a living document.
- The agent must meticulously document all findings, discoveries, challenges, and issues that arise during the release process in the `handoff_notes.md` file.

**State Transitions:**
- As the release progresses through the stages defined in the branching strategy (e.g., from `alpha` to `beta`, `beta` to `rc`), the agent must record each state change in both the `handoff_notes.md` and the main release plan document.

## 3. Best Practices for Release
- **Automate Everything:** The build, testing, and deployment process should be as automated as possible to ensure reliability, repeatability, and to reduce human error.
- **Maintain a Release Checklist:** Use a detailed, version-controlled checklist for every release to ensure that no step, from final builds to communication, is missed.
- **Staged Rollouts:** Whenever possible, use techniques like canary releases, blue-green deployments, or feature flags to gradually roll out changes. This minimizes the "blast radius" of any unforeseen issues.
- **Clear Communication:** Maintain clear and constant communication with internal stakeholders about the release schedule and progress. Prepare external communications well in advance.

## 4. The Release Lifecycle
This section outlines the standardized process for taking a completed software build and releasing it to users. The lifecycle is divided into distinct stages, each with its own set of goals and activities.

### 4.1. Release Planning
Before the release process begins, a formal plan is created. This includes defining the scope of the release, setting a target date, and assigning a version number. The initial version for a new product should start at `v0.0.1`. All versioning must be in strict accordance with Semantic Versioning (SemVer) principles. This plan governs the entire release lifecycle.

#### 4.1.1. Release Branching Strategy
The release process must follow a structured branching strategy.
- **Alpha:** `alpha/v0.0.x` - For initial, internal testing.
- **Beta:** `beta/v0.0.x` - For wider, public testing.
- **Release Candidate (RC):** `rc-X.Y/v0.0.x` - For final testing before release. `X.Y` can range from `0.0` to `9.9` as needed.
- **Release:** `release/v0.0.x` - The final, approved, and tagged version of the software.

**Note on Session Restarts:** Due to environment limitations, if a new agent session begins by cloning a repository that is already on a target branch (e.g., `alpha/v0.0.1`), the agent must create a new, session-specific branch by appending an incrementing number. For example, `alpha-1/v0.0.1`, `alpha-2/v0.0.1`, and so on.

### 4.2. Preparation Stage
This stage focuses on ensuring that all prerequisites for testing and deployment are met.

#### 4.2.1. Feature Audit
**MANDATE:** A thorough and comprehensive audit of the entire codebase MUST be performed. This audit involves a deep comparison of the implemented code against all relevant documentation, including the architecture specification, development plan, and feature checklists. The goal is to ensure that all planned features have been implemented completely and correctly, and that the final state of the code accurately reflects the design and planning documents. This is a critical verification step, not a basic sanity check.

#### 4.2.2. Environment Provisioning
Any test-related tools or dependencies, such as message brokers, databases, or specific system/network services, are developed and/or installed in the testing environment.

#### 4.2.3. Deployment Tooling
Any tools, distributables, or packages needed to support the final deployment of the application are developed and prepared during this stage.

### 4.3. Test Stage
This stage is an iterative cycle focused on producing a stable, high-quality, and user-approved release candidate. It follows the explicit workflow: **change -> local build/test -> system build -> system test**. At the beginning of this stage, a version and branch must be assigned (e.g., `alpha/v0.0.1`). **MANDATE: No step in this workflow may be skipped. The agent MUST successfully complete local verification (`cargo build` and `cargo test`) before proceeding to a full system build (`scripts/build_system.sh`) and system test (`scripts/run_system_test.sh`).**

**Environment and Dependency Setup:**
- **Primary Environment:** By default, all testing will be performed in a **direct environment** by running the application on the host machine.
- **Docker Policy:** The agent is responsible for creating and maintaining any necessary `Dockerfile` and `docker-compose.yml` files. However, the agent **MUST NOT** attempt to build Docker images itself. Docker builds are handled by the user outside the agent's environment, primarily for UAT. The agent should only expect to use a Docker environment (e.g., with `docker compose up`) when explicitly instructed by the user for a UAT scenario.
- **Setup Scripts:** The agent must ensure that the environment is correctly configured using the standardized scripts. This includes ensuring `scripts/setup_env.sh` is up-to-date and that any necessary services have been started with `scripts/start_services.sh`.

**System Test Execution:**
- **Single, Repeatable Script:** The entire system test process must be encapsulated in the single, top-level script: `scripts/run_system_test.sh`. This script must be kept repeatable and consistent.
- **Script Orchestration:** The `scripts/run_system_test.sh` script should act as an orchestrator. It is responsible for calling the build script (`scripts/build_system.sh`) and any necessary verification scripts (e.g., `scripts/verify_*.js`). This maintains a separation of concerns.
- **Pre-flight Check:** The `scripts/run_system_test.sh` script must begin by checking that all dependencies from `scripts/setup_env.sh` are installed and configured correctly before proceeding.
- **Logging Mandate:** All executed scripts and applications MUST be fully and extensively instrumented with permanent, multi-level logging, using the `tracing` framework as specified in the development guide. This instrumentation MUST NOT be removed. The system test script must capture detailed, complete logs from all steps (using all appropriate levels from Informational to Emergency) and save them to a file within the appropriate `test_outs/` subfolder. Verification of these logs for expected output is a required part of the test validation.
- **Visual Components:** If the project has a visual or client component, the `scripts/run_system_test.sh` script must orchestrate the execution of a Playwright test (e.g., `scripts/verify_ui.js`) that captures screenshots to the `test_outs/` directory.

**Iterative Test Workflow:**
1.  **Local Test Output Setup:** The agent must ensure the local (and git-ignored) test output directory `test_outs/<version>/` exists, containing a `reference/` and a `temp/` subfolder, as defined in `AGENTS.md`.
2.  **Initial Local Reference Set:** The agent's first task is to run the `scripts/run_system_test.sh` script once. The outputs are stored in the local `reference/` directory. After self-verification, the agent must present the file path to this directory to the user for initial approval.
3.  **Iterative Development & Testing:**
    - The user provides tasks for fixes or features.
    - **No Stub Implementations:** When implementing these tasks, all code MUST be fully implemented. Placeholders, stubs, `todo!` macros, or any form of partial solution are strictly forbidden, in accordance with the "Mandate for Maximal Implementation & Robustness" (`AGENTS.md`, section 2.2).
    - For each subsequent test run, the agent executes `scripts/run_system_test.sh` and saves the outputs into a new, unique, timestamped subfolder inside the local `test_outs/<version>/temp/` directory.
4.  **Final Verification and Candidate for Approval:**
    a.  **Final Test Run:** When the agent believes a task is complete, it runs `scripts/run_system_test.sh` one last time, saving the results to a new `{TIMESTAMP}` folder inside `temp/`.
    b.  **Mandatory Log Inspection:** Before presenting the results for approval, the agent MUST meticulously inspect all relevant logs to confirm the test outcome. This includes:
        - Server logs (from `tracing` instrumentation)
        - Client logs (from `tracing` instrumentation)
        - Console output from the test runner
        - Logs from any other system components
        - Browser console logs (which must be enabled and captured by the verification scripts)
    c.  **Present for Approval:** Only after a successful log inspection, the agent MUST present the **direct file path** to the final timestamped folder (e.g., `test_outs/<version>/temp/{final_timestamp}/`) to the user for review.
5.  **Approval & Commit Workflow:**
    - A commit MUST NOT be made until the user approves the candidate test results via the provided file path.
    - **Upon user approval:**
        1. The agent must copy the approved contents from the candidate folder into the local `reference/` folder, replacing the old local reference set.
        2. **Cleanup Mandate:** The agent MUST delete the entire `temp/` directory and all its contents.
        3. The agent must update `handoff_notes.md`.
        4. The agent must then **commit the source code changes only**.
6.  **Promotion to UAT & UAT Loop:**
    - User acceptance of the test outputs promotes the release to the next stage (e.g., `rc-0.1`), triggering the User Acceptance Test (UAT).
    - If UAT feedback requires changes, the project returns to step 3 of this Test Stage to begin another iteration.
    - If UAT is accepted, the project is approved to move to the `4.4. Deployment Stage`.

### 4.4. Deployment Stage
Once a release candidate has been approved, this stage manages the final packaging and rollout.

#### 4.4.1. Crate Preparation for Publishing
**MANDATE:** Before any final build or packaging, the agent MUST ensure all crates intended for publishing are correctly configured and documented for `crates.io`.

- **`Cargo.toml` Metadata:** The `[package]` section of each `Cargo.toml` MUST include the following fields, populated with appropriate content:
    - `license`
    - `description`
    - `version`
- **`Cargo.toml` Local Dependencies:** All local path dependencies MUST also have an explicit `version` key. This is required by `crates.io`.
    - **Example:** `my-local-crate = { path = "../my-local-crate", version = "0.1.0" }`
- **Crate-Level `README.md`:** Each crate directory MUST contain a `README.md` file that provides a clear and concise description of the crate's purpose and usage.

The agent must perform a check for these items and add or update them as necessary.

#### 4.4.2. Final Build & Packaging
-   **Finalize Documentation Set:** All user-facing and developer-facing documentation must be created, finalized, and incorporated into the release package. This includes:
    - **User Documentation:** User guides, tutorials, and examples.
    - **Developer Documentation:** API references, architecture documents, contribution guidelines, and code-level documentation.
    - **Release Notes:** A detailed summary of the changes in this release.
-   **Build Final Release Package:** The final, approved release candidate is built and packaged along with its comprehensive documentation set and all dependencies into the final distributable format.

#### 4.4.3. Marketing & Communications
This stage involves creating and disseminating a suite of documents to announce the release to the public and technical communities.

- **Core Announcements:** The agent must create the following three documents:
    - **Launch Announcement:** A concise, formal announcement of the new release.
    - **Technical Article:** A detailed article explaining the technical aspects of the release, its features, and architecture.
    - **General Blog Post:** A more accessible blog post summarizing the release for a general audience.

- **Targeted Outreach:**
    - The agent must identify at least 10 relevant websites, blogs, or news outlets (e.g., The Verge, Hacker News, Hackaday) that might be interested in the project.
    - The agent must present this list of proposed targets to the user for approval before proceeding.
    - Once approved, for each target, the agent must research the site's content and style.
    - The agent must then write a tailored article or blog post for each of the 10 targets, written in a style that maximizes the likelihood of publication.

#### 4.4.4. Production Rollout
The final, packaged release is deployed to the production environment, following the chosen rollout strategy (e.g., canary, full deployment).

### 4.5. Post-Release Monitoring
After deployment, the team must actively monitor system health, performance metrics, error rates, and user feedback channels (e.g., social media, issue trackers) to rapidly identify and address any issues that arise.

## 5. Quality and Completeness for Release
### 5.1. Go/No-Go Decision
A formal sign-off meeting is mandatory before deployment. Key stakeholders review the QA results, release checklist, and documentation status to make a final "Go/No-Go" decision. A release cannot proceed without this approval.

### 5.2. Documentation Mandate
A release is not considered complete until all user documentation and release notes have been finalized and published.

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-08-31 | Jules       | Initial creation of the guide.        |
