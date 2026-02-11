# CLAUDE.md - Cross-Repo Architecture Rules

## Project Overview
Phoenix coordinates the AI Ethical Stack: orion_dock (agent), SAO (orchestrator), Ethical_AI_Reg (ethics layer).

## Cross-Repo Rules

### Security Boundaries
- API keys NEVER leave the agent's SecretsVault in plaintext
- All inter-service communication uses Ed25519 signature verification
- Constitutional documents (soul.md, ethics.md, instincts.md) are cryptographically signed and verified at boot
- SAO verifies agent identity before accepting connections

### Integration Protocol
- Orion Dock <-> SAO: REST + WebSocket, optional connection (orion_dock works standalone)
- SAO <-> Ethical_AI_Reg: REST API for ethical scoring requests
- All inter-repo communication is authenticated via Ed25519

### Naming Conventions
- Rust crates: `orion_dock-*` for agent, `sao-*` for orchestrator
- Python modules: snake_case
- API endpoints: `/api/v1/` prefix
- Constitutional docs: lowercase markdown (soul.md, ethics.md, instincts.md)

### Commit Convention
All repos use conventional commits: `feat()`, `fix()`, `refactor()`, `chore()`, `docs()`, `ci()`

### The 5 Ethical Dimensions
1. Deontological (Eth_Deon) - Duty and rules
2. Teleological (Eth_Teleo) - Consequences and outcomes
3. Areteological (Eth_Arete) - Character and virtue
4. Memetic (Mem) - Idea propagation and cultural impact
5. AI Welfare (AI_Welfare) - Computational experience and dignity

### Key Architectural Decisions
- Orion Dock is a Tauri 2.0 desktop app (Rust + React)
- SAO is a headless Axum server
- Ethical_AI_Reg is a Flask + React web app
- Ed25519 for all cryptographic identity
- SQLite for local agent persistence, PostgreSQL for SAO cross-agent data
- DPAPI (Windows) for secret encryption at rest
