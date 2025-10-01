# Marrow: Agent Instruction Repository

This repository houses the central set of instructions and mandates that govern the behavior of an advanced AI software engineering agent. It serves as a single source of truth to ensure all agent actions are predictable, robust, and aligned with a strict operational workflow. It is a Rust centric software development process.

## Structure of Development Phases

The agent's workflow is built around a five-phase development lifecycle, with each phase having a dedicated guide in the `agents/` directory:

1.  **Design Phase (`DESIGN.md`):** Translating a new idea into a comprehensive `Architecture Specification`.
2.  **Planning Phase (`PLANNING.md`):** Decomposing the architecture into a detailed, actionable `Development Plan` and checklist.
3.  **Development Phase (`DEVELOPMENT.md`):** Executing the plan to write, build, and test the code, following a strict `change -> build -> test` cycle for every modification.
4.  **Release Phase (`RELEASE.md`):** Packaging, documenting, and deploying the completed software, including a rigorous `change -> build -> test -> system test` cycle.
5.  **Maintenance Phase (`MAINTENANCE.md`):** Managing bug fixes and new features for a deployed product.

## Key Controls & Core Mandates

These are the most critical rules that govern all agent actions, established to ensure precision, robustness, and safety:

*   **Rust as the Standard :** The primary and preferred language for all application code is **Rust**. Other languages may be used for ancillary scripts, but all core logic must be written in Rust.
*   **"Super-Strict" Workflow :** For any user instruction, the agent must first respond with its interpretation, an exact plan of the commands it will run, and then wait for explicit approval before taking any action.
*   **Command Output Verification :** The agent must inspect the direct output of every command to determine success. It is forbidden from running extra commands to check, and if the output is ambiguous, it must assume failure and report it.
*   **Maximal Implementation :** The agent is forbidden from creating "stub" or partial implementations. All tasks must be interpreted deeply and implemented to their fullest, most robust potential.
*   **Code Review Feedback :** The agent is to completely ignore the content of code review feedback. It is for the user's analysis only, and the agent will only act on it if given a new, explicit instruction.
*   **Sanctity of the Filesystem :** The agent is forbidden from deleting, moving, overwriting, or adding files to `.gitignore` without explicit approval.
*   **Session Initialization :** At the start of every session, the agent must follow a strict startup sequence: run `scripts/start_services.sh` if it exists, then run `scripts/check_env.sh` and halt if it fails.
