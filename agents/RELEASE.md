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

### 4.2. Preparation Stage
This stage focuses on ensuring that all prerequisites for testing and deployment are met.

#### 4.2.1. Feature Audit
A basic check of implemented functionality against the applicable architecture specification, development plan, and checklist is performed. This is a sanity check to ensure the release candidate is aligned with the project goals before intensive testing begins.

#### 4.2.2. Environment Provisioning
Any test-related tools or dependencies, such as message brokers, databases, or specific system/network services, are developed and/or installed in the testing environment.

#### 4.2.3. Deployment Tooling
Any tools, distributables, or packages needed to support the final deployment of the application are developed and prepared during this stage.

### 4.3. Test Stage
This stage is an iterative cycle of `test -> correct -> repeat` with the goal of producing a stable, high-quality, and user-approved release candidate.

1.  **Build Project:** Compile the project. The build must be error-free. Warnings should be addressed and minimized to the greatest extent possible.
    - **Environment Setup:** The agent will set up two test environments:
        - **Direct Environment:** The application is run directly on the host machine.
        - **Containerized Environment:** The application is run within a container (e.g., Docker) to validate its portability and dependency encapsulation.
    - **Documentation:** The agent will provide clear, step-by-step instructions for a user or another agent to replicate the system test in both the direct and containerized environments.
2.  **Unit Tests:** All unit tests must pass.
3.  **Integration Tests:** All integration tests must pass.
4.  **System Test:** This is a comprehensive, agent-driven test of the fully integrated system to verify it meets all specified requirements. The following verification steps must be performed in order:
    - **Health Check:** The agent must perform a basic health check using a tool like `curl`. A successful check requires not only a `200 OK` status code from a health endpoint but also verification of a specific success message in the response body (e.g., `{"status": "ok"}`).
    - **Output Verification:** The agent must verify the primary outputs of the application. The verification process is: **Capture -> Agent Review -> User Approval**. No fixes should be attempted until the user has approved the test results.
        - **For Web UIs:** Capture both a Playwright screenshot and the raw HTML/data. The agent must review both, share them and its assessment with the user, and get approval.
        - **For Non-Web Visuals (e.g., generated images):** Capture a screenshot of the output. The agent must review it, share it and its assessment with the user, and get approval.
        - **For CLI or Data Outputs:** Capture the raw text or data output. The agent must review it, share it and its assessment with the user, and get approval.
5.  **User Acceptance Test (UAT):** This test is performed *after* the System Test has been successfully completed and reviewed. The release candidate, along with its documentation and the results of the System Test (including screenshots), is packaged and delivered to the user for final approval.
6.  **User Feedback & Confirmation:** The team gathers feedback from the UAT. Any critical issues are addressed, and the cycle repeats (starting from the appropriate test stage) until the user formally approves the release candidate.

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
