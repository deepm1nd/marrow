# Preferred Dependencies

This document lists the preferred dependencies for Rust projects. When designing, planning, or developing, you must check against this list. If a dependency is not on this list, you must notify the user and get approval before using it.

## Web & Networking
- tokio
- axum
- reqwest
- tonic
- tungstenite
- tokio-tungstenite
- url
- wtransport

## Frontend (Yew)
- yew
- yew-router
- material-yew

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

## AVOID

- [No dependencies to avoid have been specified yet.]