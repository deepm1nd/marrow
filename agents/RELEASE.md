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
A basic check of implemented functionality against the applicable architecture specification, development plan, and checklist is performed. This is a sanity check to ensure the release candidate is aligned with the project goals before intensive testing begins.

#### 4.2.2. Environment Provisioning
Any test-related tools or dependencies, such as message brokers, databases, or specific system/network services, are developed and/or installed in the testing environment.

#### 4.2.3. Deployment Tooling
Any tools, distributables, or packages needed to support the final deployment of the application are developed and prepared during this stage.

### 4.3. Test Stage
> **WARNING:** Due to environment constraints, the agent **MUST NOT** attempt to build Docker containers. All testing must be performed in the local, direct environment unless a pre-built image is explicitly provided by the user.

This stage is an iterative cycle of `test -> correct -> repeat` with the goal of producing a stable, high-quality, and user-approved release candidate. At the beginning of this stage, a version and branch must be assigned (e.g., `alpha/v0.0.1`).

The workflow for this stage is as follows:

**Agent Self-Verification Mandate:** Before any output is presented to the user for approval, the agent MUST first perform its own review to check for correctness.
- **For Visual Outputs (e.g., PNG screenshots):** The agent must use the `read_image_file(filepath: str)` tool to load the image into its context. It must then visually analyze the image to confirm that the UI has rendered as expected, looks correct, and has no obvious visual bugs or defects.
- **For Data Outputs (e.g., HTML, JSON, logs):** The agent must read the file content and parse it to confirm the data is structured correctly and contains the expected information.

1.  **Test Output Setup:** The agent must create a structured directory for the test outputs corresponding to the current branch, e.g., `test_outs/v0.0.1/alpha-1/`. All subsequent test outputs for this session will be managed within this folder.
2.  **Initial System Test (Reference Set):** The agent's first task is to run the full system test suite once.
    - The outputs (logs, screenshots, etc.) are to be stored in a `reference/` subfolder within the session's test output directory.
    - After performing the **Agent Self-Verification** on the outputs, the agent must then present this reference set to the user for approval before any fixes or feature work begins.
3.  **Iterative Development & Testing:**
    - Once the reference set is approved, the user will provide tasks for fixes or features.
    - The agent will work on these tasks, running tests as needed and generating intermediate test outputs in temporary subfolders.
    - Once the agent believes a task is complete, it must run a final verification test.
    - **Cleanup:** The agent must delete all intermediate test outputs from the temporary subfolders.
    - The final outputs are stored in a `for_approval/` subfolder.
    - The agent presents the `for_approval/` outputs to the user. The user may approve them or request further changes, continuing the loop.
4.  **Promotion to UAT:**
    - At some point, the user will accept the `for_approval/` outputs as final for the current release stage. The user may at this point request that the `for_approval/` set becomes the new `reference/` set for subsequent work.
    - This acceptance promotes the release to the next stage (e.g., a release candidate, `rc-0.1`), which triggers the User Acceptance Test (UAT).
5.  **User Acceptance Test (UAT):**
    - The release candidate is delivered to the user for UAT.
    - If UAT feedback requires changes, the project returns to step 3 of this Test Stage to begin another iteration.
    - If UAT is accepted, the project is approved to move to the `4.4. Deployment Stage`.

### 4.4. Deployment Stage
Once a release candidate has been approved, this stage manages the final packaging and rollout.

#### 4.4.1. Final Build & Packaging
-   **Finalize Documentation Set:** All user-facing and developer-facing documentation must be created, finalized, and incorporated into the release package. This includes:
    - **User Documentation:** User guides, tutorials, and examples.
    - **Developer Documentation:** API references, architecture documents, contribution guidelines, and code-level documentation.
    - **Release Notes:** A detailed summary of the changes in this release.
-   **Build Final Release Package:** The final, approved release candidate is built and packaged along with its comprehensive documentation set and all dependencies into the final distributable format.

#### 4.4.2. Marketing & Communications
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

#### 4.4.3. Production Rollout
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
