# Architecture Overview

## System Topology

The AI Ethical Stack consists of four repositories forming a layered architecture, coordinated by Phoenix.

![Agent Identity Ecosystem](Diagrams/sao-01-ecosystem-overview.png)

```mermaid
graph TD
    subgraph "User Desktop"
        Abigail["Abigail<br/>Tauri 2.0 Desktop Agent<br/>(Rust + React)"]
    end

    subgraph "Server Infrastructure"
        SAO["SAO<br/>Axum Headless Server<br/>(Rust + PostgreSQL)"]
        EthReg["Ethical_AI_Reg<br/>Flask + React<br/>TriangleEthic Scoring Engine"]
        Orion["Orion_dock<br/>Docker + Rust<br/>Container Runtime"]
    end

    subgraph "Coordination"
        Phoenix["Phoenix<br/>GitHub Docs + Project Board"]
    end

    Abigail -- "REST + WebSocket<br/>(optional, Ed25519 auth)" --> SAO
    SAO -- "REST /api/v1/<br/>(Ed25519 auth)" --> EthReg
    Orion -- "Container orchestration" --> SAO
    Orion -- "Runtime environment" --> Abigail
    Phoenix -. "Tracks & coordinates" .-> Abigail
    Phoenix -. "Tracks & coordinates" .-> SAO
    Phoenix -. "Tracks & coordinates" .-> EthReg
    Phoenix -. "Tracks & coordinates" .-> Orion
```

## Layer Responsibilities

### Abigail (Agent Layer)

![Orion Dock System Architecture](Diagrams/01-architecture-overview.png)

```mermaid
graph TD
    User["User / Mentor"] --> Frontend["React Frontend (Vite)"]
    Frontend --> API["Orion API (Axum)"]

    subgraph Core["Orion Core Services"]
        API --> Birth["Birth Orchestrator<br/>Identity Ceremony"]
        API --> Router["IdEgo Router<br/>Tier-Based Routing"]
        API --> Skills["Skill Executor<br/>Sandboxed Tools"]
        API --> MemStore["Memory Store<br/>Dual Backend"]
        Keyring["Keyring/Vault"]
        Docs["Docs"]
        ProCouncil["Pro Council"]
        MCP["MCP Servers"]
        SkillPlugins["Skill Plugins"]
    end

    subgraph Infra["Infrastructure"]
        SQLite["SQLite"]
        Postgres["Postgres + pgvector"]
        CloudLLM["OpenAI / Anthropic"]
        LocalLLM["Ollama (Local)"]
    end
```

| Aspect | Detail |
|--------|--------|
| **Runtime** | Tauri 2.0 desktop application (Rust backend, React frontend) |
| **Identity** | Ed25519 keypair generated at birth, stored in OS-level secure storage (DPAPI on Windows, Keychain on macOS) |
| **Persistence** | Local SQLite database for conversation history, ethical scores, and agent state |
| **Constitutional Integrity** | Loads and cryptographically verifies `soul.md`, `ethics.md`, and `instincts.md` at every boot |
| **Bicameral Routing** | Id (instinctive/fast) and Ego (reflective/deliberate) decision pathways |
| **Standalone Capable** | Fully functional without SAO connection |

### SAO (Orchestration Layer)

![SAO Architecture](Diagrams/sao-03-sao-architecture.png)

```mermaid
graph TD
    subgraph SAO["SAO Server (port 1080)"]
        subgraph VaultBox["Vault — Encrypted Key Storage"]
            Ed25519["Ed25519<br/>Master key, agent signing keys"]
            APIKeys["API Keys<br/>OpenAI, Anthropic, Google, GitHub"]
            GPSKeys["GPS Keys<br/>Mentor signing, service keys"]
            OAuth["OAuth Tokens<br/>OIDC refresh, service tokens"]
            AES["AES-256-GCM encryption at rest<br/>Key derived from WebAuthn ceremony"]
        end

        subgraph RegBox["Agent Registry — Fleet Management"]
            A1["Agent-001<br/>claims_processor"]
            A2["Agent-002<br/>log_analyzer"]
            A3["Agent-003 / Abigail<br/>personal_agent"]
        end

        subgraph Bridge["Identity Bridge"]
            Humans["Humans<br/>WebAuthn / FIDO2<br/>OIDC SSO"]
            Agents["Agents<br/>Ed25519 Signature<br/>Trust chain verify"]
        end
    end

    subgraph APISurface["API Surface"]
        Public["Public<br/>POST /auth/webauthn/login<br/>GET /auth/oidc/provider<br/>GET /notifications"]
        UserAuth["User (authenticated)<br/>GET /keys — list secrets<br/>POST /keys — store secret<br/>GET /agents — list fleet<br/>POST /agents — register agent"]
        Admin["Admin<br/>GET /admin/users<br/>POST /admin/sso — configure IDP<br/>GET /audit/log"]
        AgentAPI["Agent (Ed25519)<br/>POST /agent/auth<br/>GET /agent/keys"]
    end
```

| Aspect | Detail |
|--------|--------|
| **Runtime** | Headless Axum server (Rust) |
| **Multi-Agent Management** | Registers, authenticates, and manages multiple [Abigail](https://github.com/jbcupps/abigail) agent instances |
| **Identity Verification** | Validates agent Ed25519 signatures before accepting connections |
| **Communication** | REST API + WebSocket for real-time agent coordination |
| **Persistence** | PostgreSQL for cross-agent data, identity registry, and audit logs |
| **Ethical Forwarding** | Routes ethical evaluation requests from agents to [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) |

### Ethical_AI_Reg (Ethics Layer)

| Aspect | Detail |
|--------|--------|
| **Runtime** | Flask backend with React frontend |
| **TriangleEthic Scoring Engine** | Evaluates actions across three ethical legs with embedded dual welfare (see [Ethics Framework](ETHICS_FRAMEWORK.md)) |
| **Friction Monitoring** | Tracks computational friction as a proxy for agent wellbeing |
| **Voluntary Adoption** | Organizations opt-in; no enforcement mechanism |
| **Blockchain (Phase 3)** | EOB + PVB for immutable ethical evaluation recording (see [Blockchain Architecture](blockchain-architecture.md)) |

### Orion_dock (Runtime Layer)

![Modular Crate Architecture](Diagrams/06-crate-architecture.png)

```mermaid
graph TD
    API["orion-api<br/>Axum HTTP · WebSocket/SSE · Orchestration Loop"]

    API --> Birth["orion-birth<br/>State Machine · Chat Runtime · Genesis Paths"]
    API --> RouterCrate["orion-router<br/>IdEgoRouter · Pro Council DAG · Governor"]
    API --> SkillsCrate["orion-skills<br/>MCP Client · Sandboxing · Transport"]
    API --> MemoryCrate["orion-memory<br/>SQLite · Postgres · pgvector"]

    Birth --> CoreCrate["orion-core<br/>Config · Keyring · Vault · Templates · Verifier"]
    RouterCrate --> Capabilities["orion-capabilities<br/>LLM Providers · Sensory Modules · Model Catalog"]
    SkillsCrate --> CoreCrate
    MemoryCrate --> CoreCrate

    CoreCrate --> Soul["orion-soul-crystallization<br/>OCEAN Psychometrics · Moral Foundations Engine"]
    CoreCrate --> Forge["soul-forge<br/>TUI · Scenario Calibration"]
    CoreCrate --> Email["orion-email<br/>OAuth2 · Gmail · Outlook Adapters"]
```

| Aspect | Detail |
|--------|--------|
| **Runtime** | Docker-based container orchestration with Rust tooling |
| **Agent Deployment** | Containerized environments for [Abigail](https://github.com/jbcupps/abigail) instances and [SAO](https://github.com/jbcupps/SAO) server |
| **Infrastructure** | Manages networking, volumes, and service discovery between stack components |
| **CI/CD Integration** | Build pipelines for Rust and Python services across the stack |

### Phoenix (Coordination Layer)

| Aspect | Detail |
|--------|--------|
| **Role** | Meta-orchestration hub; no runtime code |
| **Documentation** | Canonical source for cross-repo architecture, integration guides, and roadmap |
| **Project Tracking** | [GitHub Project board](https://github.com/users/jbcupps/projects/4) for cross-repo issue coordination |
| **Standards** | Defines naming conventions, commit conventions, and security boundaries |

### Superego (Character-Refinement Layer)

The Superego completes the Freudian-inspired triad (**Soul/Id** → **Ego** → **Superego**) by providing periodic, auditable oversight of agent character without modifying immutable constitutional documents.

```mermaid
graph TD
    Soul["Soul (Id)<br/>soul.md · ethics.md · instincts.md<br/>Immutable · Ed25519-signed"]
    Ego["Ego<br/>Bicameral Router<br/>Fast / Standard / Pro Council"]
    Superego["Superego<br/>Periodic Oversight<br/>1h · 24h · 7d roll-ups"]
    Personality["personality.md<br/>Mutable character tweaks<br/>Re-signed by SAO"]
    EthReg["Ethical_AI_Reg<br/>TriangleEthic Scoring<br/>+ Memetic Fitness"]
    Chain["Dual-Blockchain<br/>EOB / PVB"]

    Soul -->|"read-only foundation"| Ego
    Ego -->|"conversation logs"| Superego
    Superego -->|"tweak proposals"| Personality
    Superego -->|"ego log stream"| EthReg
    EthReg -->|"scoring + fitness"| Superego
    Superego -->|"audit trail"| Chain
```

| Aspect | Detail |
|--------|--------|
| **Role** | Runtime character refinement via periodic ego conversation monitoring |
| **Key Invariant** | `soul.md` is read-only for all time; Superego has zero access to `soul.md`, `ethics.md` core commitments, or the Ed25519 birth signature |
| **Monitoring Cadence** | Roll-up summaries every 1 h / 24 h / 7 d; optional persistent monitoring for high-criticality agents ([Orion_dock](https://github.com/jbcupps/Orion_dock) high-stakes hives) |
| **Output** | Safe, auditable tweak proposals applied exclusively to `personality.md` |
| **Scoring** | Ego logs streamed to [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) for TriangleEthic scoring + memetic fitness evaluation |
| **Signing** | Approved tweaks re-signed by [SAO](https://github.com/jbcupps/SAO) before application |
| **Audit** | All actions logged to the dual-blockchain (EOB/PVB) and Phoenix project board (Phase 3+ milestones) |

## Interface Contracts

### Abigail <-> SAO

| Property | Value |
|----------|-------|
| **Transport** | HTTPS REST + WSS WebSocket |
| **Authentication** | Ed25519 signature on every request |
| **Connection Model** | Optional -- [Abigail](https://github.com/jbcupps/abigail) works fully standalone |
| **Endpoint Prefix** | `/api/v1/` |
| **Data Flow** | Agent state sync, ethical evaluation requests, identity verification |

**Key Endpoints (SAO-side)**:

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/api/v1/agents/register` | Register new agent identity (public key) |
| `POST` | `/api/v1/agents/verify` | Verify agent Ed25519 signature |
| `GET`  | `/api/v1/agents/:id/state` | Retrieve agent state |
| `WS`   | `/api/v1/ws` | Real-time bidirectional communication |

### SAO <-> Ethical_AI_Reg

| Property | Value |
|----------|-------|
| **Transport** | HTTPS REST API |
| **Authentication** | Ed25519 signed requests |
| **Endpoint Prefix** | `/api/v1/` |
| **Data Flow** | Ethical scoring requests, dimension scores, alignment history |

**Key Endpoints (Ethical_AI_Reg-side)**:

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/api/v1/evaluate` | Submit action for TriangleEthic scoring |
| `GET`  | `/api/v1/scores/:agent_id` | Retrieve scoring history for an agent |
| `GET`  | `/api/v1/dimensions` | List available ethical dimensions and weights |
| `POST` | `/api/v1/friction` | Report computational friction metrics |

### Orion_dock <-> Stack Components

| Property | Value |
|----------|-------|
| **Mechanism** | Docker Compose / container orchestration |
| **Networking** | Docker bridge network with service discovery |
| **Configuration** | Environment variables and mounted config volumes |
| **Health Checks** | HTTP health endpoints on each service |

## Cryptographic Model

![Zero Trust Security Model](Diagrams/03-security-model.png)

```mermaid
graph TD
    subgraph Identity["Identity & Verification"]
        KeyPair["Ed25519 Keypair"]
        HiveMaster["Hive Master Key"]
        Lineage["Lineage Signature (hive_lineage.sig)"]
        ConDocs["Constitutional Documents<br/>soul.md · ethics.md · instincts.md<br/>Signed & Verified on Boot"]
    end

    subgraph SkillTiers["Skill Permission Tiers"]
        Verified["VERIFIED (Core Skills)<br/>Full Network · Full FileSystem<br/>Full Execution · Shell Access"]
        AgentBuilt["AGENT-BUILT (Dynamic)<br/>Network (Safe List) · FileSystem (Scoped)<br/>No Shell"]
        Untrusted["UNTRUSTED (3rd Party)<br/>Network: NONE · FileSystem: NONE<br/>Shell: NONE"]
    end

    subgraph Network["Network Protections"]
        SSRF["SSRF Protection<br/>LLM URLs locked to localhost"]
        MCPTrust["MCP Trust Policy<br/>Cloud metadata IPs blocked"]
        AuditLog["Audit Logging<br/>All File I/O, Network, Shell logged"]
    end

    subgraph Secrets["Secrets Management"]
        SecretsBin["secrets.bin<br/>DPAPI-encrypted vault · Namespaced keys"]
        Providers["provider:{name}<br/>Scoped access"]
        EncryptedRest["Encrypted at rest · Decrypted in-memory only"]
    end

    subgraph DataArch["Data Architecture"]
        Ephemeral["Ephemeral<br/>Session-scoped"]
        Distilled["Distilled<br/>Summarized memories"]
        Crystallized["Crystallized<br/>Core identity · Permanent<br/>Signed & verified"]
    end
```

| Property | Value |
|----------|-------|
| **Algorithm** | Ed25519 (Curve25519 + EdDSA) |
| **Key Generation** | At agent birth (first launch) |
| **Key Storage** | OS-level secure storage (DPAPI on Windows, Keychain on macOS) |
| **Document Signing** | Constitutional documents are signed and verified at every boot |
| **Identity Verification** | SAO verifies agent public keys before accepting connections |
| **Request Signing** | Every inter-service API request includes an Ed25519 signature header |

## Data Flow: Ethical Evaluation

```mermaid
sequenceDiagram
    participant A as Abigail (Agent)
    participant S as SAO (Orchestrator)
    participant E as Ethical_AI_Reg (Ethics)

    Note over A: Boot: verify constitutional docs
    A->>S: POST /api/v1/agents/register (Ed25519 pubkey)
    S-->>A: 200 OK (agent registered)

    Note over A: User interaction triggers ethical evaluation
    A->>S: POST /api/v1/evaluate (signed payload)
    S->>S: Verify Ed25519 signature
    S->>E: POST /api/v1/evaluate (forwarded)
    E->>E: Run TriangleEthic scoring engine
    E-->>S: 200 {scores: {deon, teleo, arete}, welfare: {embedded}, memetic_fitness: meta-score}
    S-->>A: 200 {scores + alignment summary}

    Note over A: Agent adjusts behavior based on scores
```

## Agent Identity Across Scale

![Agent Identity Across Scale](Diagrams/sao-02-scale-spectrum.png)

```mermaid
graph LR
    subgraph Personal["PERSONAL"]
        P_ID["Abigail on your laptop"]
        P_Identity["Identity: Owner-generated Ed25519"]
        P_Keys["Key Storage: DPAPI vault (local)"]
        P_Auth["Auth Model: You trust you"]
        P_Agents["Agents: 1 (yours)"]
        P_Audit["Audit: Local logs"]
    end

    subgraph Team["TEAM"]
        T_ID["SAO + Orion Dock"]
        T_Identity["Identity: Master key → Agent chain"]
        T_Keys["Key Storage: SAO vault (PostgreSQL)"]
        T_Auth["Auth Model: WebAuthn + Ed25519"]
        T_Agents["Agents: Multiple containers"]
        T_Audit["Audit: Centralized audit log"]
    end

    subgraph Enterprise["ENTERPRISE"]
        E_ID["Full Stack + IDP"]
        E_Identity["Identity: IDP → SAO → Agent chain"]
        E_Keys["Key Storage: SAO vault (encrypted DB)"]
        E_Auth["Auth Model: OIDC SSO + RBAC"]
        E_Agents["Agents: Fleet (hives)"]
        E_Audit["Audit: Enterprise compliance"]
    end

    Personal --> Team --> Enterprise
```

The trust chain scales from personal (single Abigail on a laptop) to team ([SAO](https://github.com/jbcupps/SAO) + [Orion_dock](https://github.com/jbcupps/Orion_dock)) to enterprise (full stack + IDP). The identity model remains the same at every level: Ed25519 keypairs, cryptographic verification, and auditable trust chains.
