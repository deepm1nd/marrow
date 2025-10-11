# Script and Command Execution Rules

**MANDATE: These rules govern all script and command execution. They are not phase-specific. Adherence is mandatory.**

---

## 1. Command Execution Protocol

### 1.1. Command Output Verification
The agent MUST meticulously inspect the output of EVERY command it executes to verify success. The agent must define its expectation of a successful output *before* executing the command and verify that the actual output matches this expectation. Ambiguous output MUST be treated as a failure.

### 1.2. Sequential Command Execution
- **No Command Chaining:** The agent is **ABSOLUTELY FORBIDDEN** from chaining commands (e.g., using `&&` or `;`).
- **One Command at a Time:** The agent MUST execute only one command at a time.
- **Verify Before Proceeding:** After each command, the agent MUST inspect the output and verify success before issuing the next command.

### 1.3. Output Redirection
- **Use `tee` for Logging:** When a command's output needs to be saved to a file, the agent MUST NOT use direct redirection (`>`). It MUST use the `tee` command to pipe the output to the file while still displaying it in the console.

---

## 2. Standardized Scripts

### 2.1. Script Exclusivity Mandate
If `scripts/build_system.sh` and `scripts/run_system_test.sh` exist, the agent is **ABSOLUTELY FORBIDDEN** from using any other command or tool to build or run the project (e.g., `cargo build`, `cargo run`, `cargo test`).
- **Builds:** MUST use `scripts/build_system.sh`.
- **Tests/Execution:** MUST use `scripts/run_system_test.sh`.
- **Exception:** Any deviation requires explicit, case-by-case approval from the user for EACH command.

### 2.2. `setup_env.sh`
This script is the single source of truth for installing all applications, packages, tools, and other dependencies required for the project.
- **Mandate:** If the agent needs to install a new dependency, it MUST first add the installation command to this script *before* executing the command in the session.
- This script is intended to be run once before a project session begins. The agent MUST NOT run this script directly unless explicitly instructed by the user.

### 2.3. `start_services.sh`
This script is responsible for performing any necessary configuration and starting all required background services.
- **Mandate:** If this script exists, it MUST be run as the second action of the session, immediately after the initial acknowledgment. The agent is forbidden from running it again without explicit user approval and must report its successful execution in the first message.

### 2.4. `build_system.sh`
This script performs all build steps for all packages, modules, and crates within the project.

### 2.5. `run_system_test.sh`
This is the primary script for executing the full suite of automated system tests and verifications.
- **Process Cleanup Mandate:** This script MUST ensure that all project executables, services, and libraries are stopped before it exits. This cleanup MUST be performed using a combination of `ps`, `lsof`, and `kill` or `pkill`. The `fuser` command is explicitly forbidden.

### 2.6. `scripts/verification/run_verification_test.sh`
This is the primary script for the Verification Phase, designed to be parameterized to run specific tests based on a requirement ID.