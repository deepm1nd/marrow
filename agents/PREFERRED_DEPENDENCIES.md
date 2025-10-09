# Mandated and Forbidden Dependencies

**MANDATE: Adherence to this guide is not optional. It is a strict requirement.**

- **Preferred Dependencies:** You SHOULD prioritize using dependencies from the preferred lists below.
- **Unlisted Dependencies:** If a desired dependency is NOT on the preferred list, you MUST follow the "Dependency and Tool Selection" mandate in `AGENTS.md` to get explicit user approval before proceeding.
- **Forbidden Dependencies:** You are **ABSOLUTELY FORBIDDEN** from using any dependency on the forbidden list. There are no exceptions. Proposing or using a forbidden dependency is a critical process failure.

## Web & Networking
- tokio  # MANDATE: For native applications ONLY. Incompatible with WASM.
- axum
- reqwest
- tonic
- tungstenite
- tokio-tungstenite
- url
- wtransport

## Frontend (Yew)
# NOTE: The `tokio` runtime MUST NOT be used in WASM-based frontend applications.
- yew
- yew-router
- material-yew = { git = "https://github.com/constructableconcepts/material-yew", branch = "update-yew-0.21" } # Fork compatible with Yew 0.21.0

## WASM & Gloo
- wasm-bindgen
- wasm-bindgen-futures
- gloo-file
- gloo-net
- gloo-console
- gloo-timers
- gloo-utils
- gloo-storage

## Data & Serialization
- serde
- serde_json
- bincode
- prost

## Database & Clients
- neo4j
- qdrant-client
- rdkafka

## Async
- async-trait
- futures
- futures-util

## Error Handling
- anyhow
- thiserror

## Logging
- tracing

## Utilities
- rand
- jsonwebtoken
- chrono
- uuid
- dotenv
- base64
- regex
- once_cell
- bcrypt

## Docker
- bollard

## Forbidden Dependencies
- trunk