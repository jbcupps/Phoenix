# Architecture Overview

## System Topology

The AI Ethical Stack consists of three repositories that form a layered architecture:

```
  User Desktop                       Server Infrastructure
 +--------------+                   +------------------------+
 |              |   REST/WS         |                        |
 |  Orion Dock  | ----------------> |         SAO            |
 |  (Tauri 2.0) |   (optional)      |  (Axum + PostgreSQL)   |
 |              |                   |                        |
 +--------------+                   +----------+-------------+
                                               |
                                               | REST API
                                               v
                                    +------------------------+
                                    |                        |
                                    |    Ethical_AI_Reg       |
                                    |  (Flask + React)       |
                                    |                        |
                                    +------------------------+
```

## Layer Responsibilities

### Orion Dock (Agent Layer)
- **Runtime**: Tauri 2.0 desktop application (Rust backend, React frontend)
- **Identity**: Ed25519 keypair generated at birth, stored in OS-level secure storage (DPAPI on Windows)
- **Persistence**: Local SQLite database for conversation history, ethical scores, and agent state
- **Constitutional Integrity**: Loads and cryptographically verifies soul.md, ethics.md, and instincts.md at every boot
- **Bicameral Routing**: Id (instinctive/fast) and Ego (reflective/deliberate) decision pathways
- **Standalone Capable**: Fully functional without SAO connection

### SAO (Orchestration Layer)
- **Runtime**: Headless Axum server (Rust)
- **Multi-Agent Management**: Registers, authenticates, and manages multiple Orion Dock agent instances
- **Identity Verification**: Validates agent Ed25519 signatures before accepting connections
- **Communication**: REST API + WebSocket for real-time agent coordination
- **Persistence**: PostgreSQL for cross-agent data, identity registry, and audit logs
- **Ethical Forwarding**: Routes ethical evaluation requests from agents to Ethical_AI_Reg

### Ethical_AI_Reg (Ethics Layer)
- **Runtime**: Flask backend with React frontend
- **5D Scoring Engine**: Evaluates actions across five ethical dimensions
  - Deontological (Eth_Deon): Rule-based duty assessment
  - Teleological (Eth_Teleo): Consequence-based outcome evaluation
  - Areteological (Eth_Arete): Character and virtue alignment
  - Memetic (Mem): Idea propagation and cultural impact analysis
  - AI Welfare (AI_Welfare): Computational experience and dignity consideration
- **Friction Monitoring**: Tracks computational friction as a proxy for agent wellbeing
- **Voluntary Adoption**: Organizations opt-in; no enforcement mechanism

## Communication Protocols

### Orion Dock <-> SAO
- **Transport**: HTTPS REST + WSS WebSocket
- **Authentication**: Ed25519 signature on every request
- **Connection Model**: Optional -- Orion Dock works fully standalone
- **Data Flow**: Agent state sync, ethical evaluation requests, identity verification

### SAO <-> Ethical_AI_Reg
- **Transport**: HTTPS REST API
- **Endpoint Prefix**: /api/v1/
- **Data Flow**: Ethical scoring requests, dimension scores, alignment history

## Cryptographic Model
- **Algorithm**: Ed25519 (Curve25519 + EdDSA)
- **Key Generation**: At agent birth (first launch)
- **Key Storage**: OS-level secure storage (DPAPI on Windows, Keychain on macOS)
- **Document Signing**: Constitutional documents are signed and verified at every boot
- **Identity Verification**: SAO verifies agent public keys before accepting connections
