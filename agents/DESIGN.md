# Design Phase Guide
v.0.0.01

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Goal](#2-goal)
- [3. Best Practices for Design](#3-best-practices-for-design)
- [4. Recommended Outline for an Architecture Specification](#4-recommended-outline-for-an-architecture-specification)
  - [4.1. Introduction](#41-introduction)
  - [4.2. Product & User Requirements](#42-product--user-requirements)
  - [4.3. System Architecture](#43-system-architecture)
  - [4.4. External Interfaces & Integrations](#44-external-interfaces--integrations)
  - [4.5. Constraints & Assumptions](#45-constraints--assumptions)
  - [4.6. Verification & Validation](#46-verification--validation)
  - [4.7. Appendices (Optional)](#47-appendices-optional)
- [Appendix R - Revision History](#appendix-r---revision-history)

---

## 1. Introduction
This guide outlines the Design Phase, where the technical vision for a system is established and documented.

## 2. Goal
The goal of this phase is to produce a comprehensive **Architecture Specification**. This document serves as the single source of truth for the system's design and is the primary input for the Planning Phase.

### 2.1. Agent Workflow
**MANDATE:** The agent's workflow for the Design Phase is as follows:
1.  **Create the Architecture Specification:** The agent will create the initial `*_architecture_specification.md` document, following the outline in this guide.
2.  **Perform Critical Self-Review:** After creating the initial draft, the agent MUST perform a second, critical review of the document. The agent must ask itself the following question:
    > Is the architectural specification of the new features detailed enough for a software developer to plan and execute with a high degree of confidence of achieving the desired functionality?
3.  **Apply Core Mandates:** During this review, the agent MUST strictly apply the "Mandate for Maximal Implementation & Robustness" (`AGENTS.md`, section 2.2). The specification must be implemented to its fullest, most robust, and most complete potential.
4.  **Revise and Finalize:** Based on the self-review, the agent will revise the specification to address any gaps in detail, clarity, or robustness before presenting the final document to the user.

## 3. Best Practices for Design
- **Iterate and Collaborate:** Design is not a solo activity. Collaborate with other engineers, stakeholders, and the user to refine the architecture. Use techniques like whiteboarding and workshops.
- **Be Explicit:** Document everything clearly. Avoid ambiguity. The goal is to create a specification that is easy to understand and implement.
- **Focus on the "Why":** Don't just document the "what" and "how." Use Architecture Decision Records (ADRs) to capture the "why" behind your decisions. This is crucial for future maintainability.
- **Use Visuals:** Diagrams are essential for communicating complex ideas simply. Use the C4 model and UML diagrams as recommended in this guide.

## 4. Recommended Outline for an Architecture Specification
The following outline should be used as the structure for any `*_architecture_specification.md` document created during the Design Phase.

### 4.1. Introduction
#### 4.1.1. Document Purpose and Audience: Explains the document's role and who should read it.
#### 4.1.2. Product/System Overview: A brief, high-level summary of the software product or system.
#### 4.1.3. Problem Statement & Vision: What problem the product solves and its long-term aspirations.
#### 4.1.4. Goals & Objectives: Specific, measurable goals for the product and the system.
#### 4.1.5. Definitions, Acronyms, and Abbreviations: Glossary of terms used throughout the document.
#### 4.1.6. References: Links to related documents (e.g., market research, competitive analysis).

### 4.2. Product & User Requirements
#### 4.2.1. Target Audience & User Personas: Detailed profiles of key users and their needs.
#### 4.2.2. User Scenarios / Use Cases: Descriptions of how users will interact with the system to achieve their goals.
#### 4.2.3. Core Functional Requirements: Detailed list of what the system must do, often expressed as user stories with acceptance criteria.
#### 4.2.4. Non-Functional Requirements (Product-Level): Expectations for performance (e.g., response times), security (e.g., data privacy), usability (e.g., ease of learning), reliability, and scalability from a user/business perspective.
#### 4.2.5. Out-of-Scope Features: Features explicitly not being built in this iteration or project.
#### 4.2.6. User-Centric Acceptance Criteria: For each User Scenario / Use Case, a list of high-level, user-focused acceptance criteria must be defined. These criteria answer the question: "How do we know this feature is working correctly from a user's perspective?" and form the basis for system-level tests. They should be written in a Gherkin-style format (Given/When/Then) where possible.

### 4.3. System Architecture
#### 4.3.1. Architectural Goals & Constraints: Drivers influencing the technical design (e.g., high availability, low latency, cost-effectiveness).
#### 4.3.2. Architectural Principles: Underlying design philosophies (e.g., modularity, loose coupling, microservices, event-driven).
#### 4.3.3. System Context Diagram: A C4 Level 1 diagram showing the system's boundaries, its users (actors), and its interactions with other systems.
#### 4.3.4. Modular Decomposition Diagram: C4 Level 2 (Container) and Level 3 (Component) diagrams that break down the system into its major building blocks and show their responsibilities and interactions.
#### 4.3.5. Logical View (Component Diagram): Major components/modules of the system, their responsibilities, and how they relate to each other.
#### 4.3.6. Process View (Runtime/Concurrency Diagram): How components interact at runtime, focusing on processes, threads, and concurrency.
#### 4.3.7. Physical View (Deployment Diagram): The mapping of software components to hardware and network infrastructure.
#### 4.3.8. Data View (High-Level Schema & Data Flow): Major data entities, data stores, and the flow of data through the system.
#### 4.3.9. Data Models: A description of the system's data, including Conceptual, Logical, and Physical models.
#### 4.3.10. Key Architectural Decisions & Rationale: Documentation of significant technical choices (e.g., specific technologies, design patterns) and the reasoning behind them, including alternatives considered.
#### 4.3.11. Architecture Decision Records (ADRs): A log of all significant architectural decisions made for the project. Each ADR should capture the context, decision, and consequences.
#### 4.3.12. Paths Not Taken: As part of the ADRs, this section explicitly documents alternative solutions that were considered and the reasons they were rejected.
#### 4.3.13. Technical Non-Functional Requirements: Detailed technical specifications for performance (e.g., specific latency targets, throughput metrics), security (e.g., encryption algorithms, authentication protocols), scalability (e.g., horizontal scaling strategy), reliability, and maintainability.
#### 4.3.14. User Experience (UX) & User Interface (UI) Design
(Include this section for projects containing a UI/UX element)
##### 4.3.14.1. Key User Journeys / Workflows: Diagrams illustrating critical user paths through the application.
##### 4.3.14.2. High-Level Wireframes / Mockups: Visual representations of key screens and their layouts.
##### 4.3.14.3. Interaction Design Overview: General principles for how users will interact with the interface.
##### 4.3.14.4. Visual Design & Style Guide (Overview): Key elements of the visual design, such as color palette, typography, and iconography, or a reference to a detailed style guide.
#### 4.3.15. Component Responsibility Collaborator (CRC) Cards: A collection of CRC cards for the key components, detailing their responsibilities and collaborators.
#### 4.3.16. Sequence Diagrams: UML Sequence Diagrams for key scenarios to illustrate how components interact to fulfill a use case.
#### 4.3.17. Logging and Monitoring
- **Logging Strategy:** This section must detail the system-wide logging strategy.
  - **Log Levels:** The design must incorporate an 8-level logging system: Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug.
  - **Default Level:** The default logging level at application launch must be `NOTICE`.
  - **Configuration:** The logging level must be configurable at launch (e.g., via environment variables or command-line arguments).
  - **Output:** Logs should be directed to the console (stdout/stderr).
  - **Formatting:** Where possible, logs should be color-coded by severity level for improved readability.
- **Monitoring Strategy:** Outline the approach for monitoring the system's health and performance, including key metrics to track.

### 4.4. External Interfaces & Integrations
#### 4.4.1. External System Interfaces: Descriptions of how the system interacts with external systems or APIs.
#### 4.4.2. Third-Party Integrations: Details on any third-party services or libraries used.
#### 4.4.3. API Specifications (External): High-level overview or references to detailed API documentation for external consumers.

### 4.5. Constraints & Assumptions
#### 4.5.1. Technical Constraints: Limitations imposed by existing infrastructure, technology, or budget.
#### 4.5.2. Business Constraints: Limitations from business rules, legal, or compliance requirements.
#### 4.5.3. Assumptions: Factors believed to be true for the project to succeed.
#### 4.5.4. Dependencies: External factors or other projects that this project relies upon.

### 4.6. Verification & Validation
#### 4.6.1. Acceptance Criteria: Specific conditions that must be met for a feature or the product to be considered complete.
#### 4.6.2. Testability Considerations: How the architecture and design facilitate automated testing.

### 4.7. Appendices (Optional)
#### 4.7.1. Glossary of Terms
#### 4.7.2. Detailed Diagrams (e.g., sequence diagrams, class diagrams)

---

## Appendix R - Revision History
| Version | Date       | Author      | Changes                               |
|---------|------------|-------------|---------------------------------------|
| 0.0.01  | 2025-07-30 | Jules       | Initial creation of the guide.        |
