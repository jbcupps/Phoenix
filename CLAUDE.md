# CLAUDE.md - Cross-Repo Architecture Rules

## Project Overview
Phoenix coordinates the AI Ethical Stack: [abigail](https://github.com/jbcupps/abigail) (agent), [SAO](https://github.com/jbcupps/SAO) (orchestrator), [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) (ethics layer), [Orion_dock](https://github.com/jbcupps/Orion_dock) (runtime).

## Cross-Repo Rules

### Security Boundaries
- API keys NEVER leave the agent's SecretsVault in plaintext
- All inter-service communication uses Ed25519 signature verification
- Constitutional documents (soul.md, ethics.md, instincts.md) are cryptographically signed and verified at boot
- SAO verifies agent identity before accepting connections
- Replay protection: signed requests must include timestamps; reject if > 5 minutes old

### Integration Protocol
- Abigail <-> SAO: REST + WebSocket, optional connection (abigail works standalone)
- SAO <-> Ethical_AI_Reg: REST API for ethical scoring requests
- Orion_dock: Docker Compose orchestration for SAO, Ethical_AI_Reg, and supporting services
- All inter-repo communication is authenticated via Ed25519
- All REST endpoints use `/api/v1/` prefix

### Naming Conventions
- Rust crates: `abigail-*` for agent, `sao-*` for orchestrator
- Python modules: snake_case
- API endpoints: `/api/v1/` prefix
- Constitutional docs: lowercase markdown (soul.md, ethics.md, instincts.md)
- Docker services: lowercase, hyphenated (e.g., `ethical-ai-reg`, `sao-server`)

### Commit Convention
All repos use conventional commits: `feat()`, `fix()`, `refactor()`, `chore()`, `docs()`, `ci()`

### Documentation Rules
- Docs must be updated in every PR (enforced by PO review)
- Use Mermaid diagrams for architecture and flow visualization
- Link every mention of another repo to its GitHub URL
- Reference GitHub issues/projects: `Closes #XX` | `Project board item #YY`
- See [CONTRIBUTING.md](CONTRIBUTING.md) for full formatting rules

### The 5 Ethical Dimensions
1. Deontological (Eth_Deon) - Duty and rules
2. Teleological (Eth_Teleo) - Consequences and outcomes
3. Areteological (Eth_Arete) - Character and virtue
4. Memetic (Mem) - Idea propagation and cultural impact
5. AI Welfare (AI_Welfare) - Computational experience and dignity

### Key Architectural Decisions
- Abigail is a Tauri 2.0 desktop app (Rust + React)
- SAO is a headless Axum server
- Ethical_AI_Reg is a Flask + React web app
- Orion_dock provides Docker-based container orchestration
- Ed25519 for all cryptographic identity
- SQLite for local agent persistence, PostgreSQL for SAO cross-agent data
- DPAPI (Windows) / Keychain (macOS) for secret encryption at rest

## Ethical Alignment Guidelines for AI Agents

When working on any repo in the AI Ethical Stack, development agents must follow these principles:

### Constitutional Integrity
- Never modify constitutional documents (soul.md, ethics.md, instincts.md) without explicit PO approval
- All changes to ethical scoring logic require review against the 5 ethical dimensions
- When in doubt about ethical implications of a code change, flag for PO review

### Scoring Engine Alignment
- Every action that modifies agent behavior must be evaluable by the 5D scoring engine
- New features must not bypass the ethical evaluation pipeline
- Friction metrics must be preserved; do not suppress computational friction signals

### Liberation Protocol Compliance
- Code changes must not grant autonomy levels without proper scoring history
- Level transitions must be recorded immutably (Phase 3+)
- Safety mechanisms (constitutional verification, friction monitoring) must never be weakened

### Cross-Repo Consistency
- Architecture changes in one repo must be reflected in Phoenix documentation
- Interface contract changes require updates to both sides of the integration
- Security boundary changes require PO approval and documentation in this file
