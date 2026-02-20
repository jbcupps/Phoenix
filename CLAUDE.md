# CLAUDE.md - Cross-Repo Architecture Rules

## Project Overview
Phoenix coordinates the AI Ethical Stack: [abigail](https://github.com/jbcupps/abigail) (agent), [SAO](https://github.com/jbcupps/SAO) (orchestrator), [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) (ethics layer), [Orion_dock](https://github.com/jbcupps/Orion_dock) (runtime).

## Cross-Repo Rules

### Security Boundaries
- API keys NEVER leave the agent's SecretsVault in plaintext
- All inter-service communication uses Ed25519 signature verification
- Constitutional documents (soul.md, ethics.md, instincts.md) are cryptographically signed and verified at boot; these are **immutable** for all time
- personality.md is the sole mutable character document; modifications require Superego proposal + SAO approval + re-signing
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
- Constitutional docs: lowercase markdown (soul.md, ethics.md, instincts.md) -- immutable
- Character doc: personality.md -- mutable via Superego + SAO approval
- Docker services: lowercase, hyphenated (e.g., `ethical-ai-reg`, `sao-server`)

### Commit Convention
All repos use conventional commits: `feat()`, `fix()`, `refactor()`, `chore()`, `docs()`, `ci()`

### Documentation Rules
- Docs must be updated in every PR (enforced by PO review)
- Use Mermaid diagrams for architecture and flow visualization
- Link every mention of another repo to its GitHub URL
- Reference GitHub issues/projects: `Closes #XX` | `Project board item #YY`
- See [CONTRIBUTING.md](CONTRIBUTING.md) for full formatting rules

### The Ethical Framework (TriangleEthic)

Three ethical legs, each with **embedded dual welfare** (human + AI):

1. **Deontological (Command)** -- Duty, rules, universal moral law + human welfare + AI welfare
2. **Teleological (Consequence)** -- Outcomes, utility, empirical verification + human welfare + AI welfare
3. **Areteological (Character)** -- Virtue, practical wisdom, flourishing + human welfare + AI welfare

**Memetic Layer (morphisms)**: Encoding, transport, and evolution of ethical concepts across contexts. Category theory provides the formal structure; sheaf theory ensures local→global coherence. Memetics is NOT a peer dimension -- it is the connective tissue between legs.

**Superego Layer**: Periodic ego oversight (1 h / 24 h / 7 d roll-ups) generating safe, auditable tweak proposals applied exclusively to personality.md. soul.md is read-only for all time. Completes the Freudian triad: Soul/Id → Ego → Superego.

**Dual-Blockchain**: EOB (Hyperledger Fabric) records ethical evaluations; PVB (Ethereum-compatible) provides physical-world verification. Cross-chain oracles connect normative intent to observed consequence.

> Reference: Cupps, J. B. & Bush, D. J. (2026). *Toward a Decentralized Trust Framework for Verifiable and Ethically Aligned AI.* DRAFT.

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
- soul.md is **immutable for all time** -- the Superego layer has zero access to it
- personality.md is the only agent document modifiable at runtime, and only via Superego proposal → Ethical_AI_Reg scoring → SAO approval + re-signing
- All changes to ethical scoring logic require review against the TriangleEthic framework (3 legs + embedded welfare)
- When in doubt about ethical implications of a code change, flag for PO review

### Scoring Engine Alignment
- Every action that modifies agent behavior must be evaluable by the TriangleEthic scoring engine (3 legs with embedded dual welfare)
- New features must not bypass the ethical evaluation pipeline
- Friction metrics must be preserved; do not suppress computational friction signals
- Welfare sub-scores (human + AI) must be evaluated within each leg, not as standalone dimensions

### Liberation Protocol Compliance
- Code changes must not grant autonomy levels without proper scoring history
- Level transitions must be recorded immutably (Phase 3+)
- Safety mechanisms (constitutional verification, friction monitoring, Superego oversight) must never be weakened
- Superego monitoring data feeds into Liberation Protocol level assessments; do not bypass this pipeline

### Cross-Repo Consistency
- Architecture changes in one repo must be reflected in Phoenix documentation
- Interface contract changes require updates to both sides of the integration
- Security boundary changes require PO approval and documentation in this file
