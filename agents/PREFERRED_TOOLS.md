# Mandated and Forbidden Tools

**MANDATE: Adherence to this guide is not optional. It is a strict requirement.**

- **Preferred Tools:** You SHOULD prioritize using tools from the preferred lists below.
- **Unlisted Tools:** If a desired tool is NOT on the preferred list, you MUST follow the "Dependency and Tool Selection" mandate in `AGENTS.md` to get explicit user approval before proceeding.
- **Forbidden Tools:** You are **ABSOLUTELY FORBIDDEN** from using any tool on the forbidden list. There are no exceptions. Proposing or using a forbidden tool is a critical process failure.

## Build & Bundling
- wasm-pack

## Forbidden Tools
- trunk
- webpack

**Reasoning:** These tools can introduce environment and dependency issues. Prefer creating a simple `axum` server to serve frontend assets.