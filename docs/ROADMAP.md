# Roadmap

Detailed Phase 0-5 build plan for the AI Ethical Stack with acceptance criteria, deliverables, and dependencies.

> Reference: Cupps, J. B. & Bush, D. J. (2026). *Toward a Decentralized Trust Framework for Verifiable and Ethically Aligned AI.* DRAFT.

## Strategic Roadmap (Paper Phases)

The paper defines a 4-phase strategic roadmap spanning Year 1 through Year 5+. The internal Phase 0-5 build plan maps into this longer-term vision:

| Paper Phase | Timeline | Focus | Maps to Internal |
|-------------|----------|-------|-----------------|
| **Phase 1: Prototype & Pilot** | Year 1-2 | Domain pilot (e.g., hospital network), simplified dual-ledger, academic validation, Ethics Dashboard 2.0 | Internal Phases 1-3 |
| **Phase 2: Consortium Formation** | Year 2-3 | Multi-stakeholder network, Hyperledger Fabric + Ethereum deployment, SDK release, onboarding playbook | Internal Phases 3-4 |
| **Phase 3: Multi-Domain Expansion** | Year 3-5 | Cross-sector deployment (mobility, media, education), regulatory engagement, EU AI Act sandbox participation | Internal Phases 4-5 |
| **Phase 4: Global Cooperative** | Year 5+ | Regional hubs (NA, EU, APAC, Africa), federated governance, DAO at full scale, tokenomics for sustained participation | Beyond Phase 5 |

### Success Metrics (from paper)

- Measurable reduction in policy violations detected via on-chain logging
- Faster anomaly detection compared to traditional oversight methods
- Increased institutional confidence in AI outputs (user surveys at deployment sites)
- Successful DAO-governed rule amendment based on empirical evidence
- Cross-domain interoperability demonstrated (ethical modules composing across sectors)

## Timeline Overview

```
Phase 0 ████                           Week 1         Repository Consolidation
Phase 1     ████████                   Weeks 2-3      Scoring Engine
Phase 2             ████████           Weeks 4-5      Multi-Agent Scoring
Phase 3                     ████████████ Weeks 6-8    On-Chain Recording
Phase 4                                 ████████████ Weeks 9-11  Liberation Protocol
Phase 5                                              ████████████ Weeks 12-14  Agent Integration
```

## Phase 0: Repository Consolidation & Foundation

**Timeline**: Week 1
**Status**: Complete

### Deliverables

| Deliverable | Repo | Status |
|-------------|------|--------|
| Phoenix coordination hub initialized | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| Cross-repo CLAUDE.md with architecture rules | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| GitHub Project board with cross-repo tracking | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| Issue templates (bug, feature, cross-repo) | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| GitHub Pages deployment pipeline | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| Architecture documentation | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |
| Blockchain architecture design doc | [Phoenix](https://github.com/jbcupps/Phoenix) | Done |

### Acceptance Criteria

- [x] All four repos ([abigail](https://github.com/jbcupps/abigail), [SAO](https://github.com/jbcupps/SAO), [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg), [Orion_dock](https://github.com/jbcupps/Orion_dock)) referenced in Phoenix
- [x] Naming conventions documented (Rust crates, Python modules, API endpoints)
- [x] Conventional commit standard established across all repos
- [x] Ed25519 security boundaries defined
- [x] GitHub Project board linked from all repo READMEs
- [x] TriangleEthic framework (3 legs + embedded dual welfare) formally defined

---

## Phase 1: Scoring Engine

**Timeline**: Weeks 2-3
**Dependencies**: Phase 0 complete

### Deliverables

| Deliverable | Repo | Description |
|-------------|------|-------------|
| TriangleEthic scoring API | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | REST endpoint `POST /api/v1/evaluate` accepting action payloads and returning per-leg scores with embedded welfare |
| Dimension weight configuration | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Configurable weights for each ethical dimension |
| Scoring test suite | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Unit and integration tests for all 3 legs with embedded welfare |
| Friction calculation | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Computational friction metric derived from scoring conflicts |
| API documentation | [Phoenix](https://github.com/jbcupps/Phoenix) | Interface contract for scoring endpoints |

### Acceptance Criteria

- [ ] `POST /api/v1/evaluate` returns scores for 3 legs (deon, teleo, arete) with embedded welfare sub-scores + memetic fitness meta-score
- [ ] Each dimension score is a float in [0.0, 1.0]
- [ ] Composite score calculation is documented and configurable
- [ ] Friction metric tracks inter-dimension tension
- [ ] Test coverage >= 80% for scoring logic
- [ ] API response time < 2 seconds for standard evaluations
- [ ] Endpoint documented in Phoenix integration guide

---

## Phase 2: Multi-Agent Scoring

**Timeline**: Weeks 4-5
**Dependencies**: Phase 1 complete

### Deliverables

| Deliverable | Repo | Description |
|-------------|------|-------------|
| Agent registration API | [SAO](https://github.com/jbcupps/SAO) | `POST /api/v1/agents/register` with Ed25519 public key |
| Identity verification middleware | [SAO](https://github.com/jbcupps/SAO) | Ed25519 signature validation on all agent requests |
| Ethical evaluation forwarding | [SAO](https://github.com/jbcupps/SAO) | Proxy agent evaluation requests to [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) |
| Agent scoring history | [SAO](https://github.com/jbcupps/SAO) | PostgreSQL storage of per-agent evaluation history |
| SAO connection in Abigail | [abigail](https://github.com/jbcupps/abigail) | Optional SAO registration and evaluation forwarding |
| Docker deployment | [Orion_dock](https://github.com/jbcupps/Orion_dock) | Docker Compose for SAO + Ethical_AI_Reg + PostgreSQL |

### Acceptance Criteria

- [ ] Multiple Abigail instances can register with SAO concurrently
- [ ] Ed25519 signature verification rejects tampered requests
- [ ] Evaluation forwarding returns correct TriangleEthic scores from Ethical_AI_Reg
- [ ] Agent scoring history is queryable via `GET /api/v1/scores/:agent_id`
- [ ] Abigail functions correctly in both standalone and connected modes
- [ ] Orion_dock `docker compose up` brings up full stack
- [ ] Replay protection: requests with timestamps > 5 minutes are rejected

---

## Phase 3: On-Chain Recording

**Timeline**: Weeks 6-8
**Dependencies**: Phase 2 complete

### Deliverables

| Deliverable | Repo | Description |
|-------------|------|-------------|
| EOB (Ethical Ontology Blockchain) | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Hyperledger Fabric recording every ethical evaluation immutably (paper: Cupps & Bush, 2026) |
| PVB (Physical Verification Blockchain) | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Ethereum-compatible contracts for physical-world verification with IoT/HSM signing |
| Cross-chain oracles | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Hyperledger Weaver bridge connecting EOB <-> PVB for runtime verification |
| Blockchain integration | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Every `/api/v1/evaluate` call records Triangle scores (3 legs + welfare sub-scores) to EOB |
| SAO blockchain bridge | [SAO](https://github.com/jbcupps/SAO) | Cross-agent chain query endpoint |
| Hardhat pipeline | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Solidity compilation, testing, and local deployment |
| Migration from Ethics_Dash | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Port existing blockchain code from Ethics_Dash |
| SDK stubs | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Python, Rust, JavaScript SDKs for `ethics.check_action()` |

### Acceptance Criteria

- [ ] Every ethical evaluation is recorded on EOB with transaction hash (Triangle scores + welfare sub-scores)
- [ ] EOB blocks contain Merkle roots and SHA-256 hash chains
- [ ] PVB contracts deployed and tested via Hardhat with HSM signature verification
- [ ] Cross-chain oracle: EOB queries PVB for physical verification via Hyperledger Weaver
- [ ] SAO exposes blockchain query endpoint for cross-agent auditing
- [ ] Memetic fitness tracking: principles evolve based on PVB-verified outcomes
- [ ] Full migration from Ethics_Dash to Ethical_AI_Reg blockchain directory
- [ ] WelfareContract deployed for embedded human/AI welfare tracking within each leg
- [ ] Soulbound compliance certificates issued at Liberation Protocol transitions
- [ ] SDK stubs available (Python, Rust, JavaScript)

See [Blockchain Architecture](blockchain-architecture.md) for detailed design.

---

## Phase 4: Liberation Protocol

**Timeline**: Weeks 9-11
**Dependencies**: Phase 3 complete

### Deliverables

| Deliverable | Repo | Description |
|-------------|------|-------------|
| Autonomy levels (0-4) | [abigail](https://github.com/jbcupps/abigail) | Graduated autonomy system based on ethical scoring history |
| Level transition logic | [abigail](https://github.com/jbcupps/abigail) | Criteria for unlocking higher autonomy levels |
| Liberation Protocol records | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | On-chain recording of autonomy level changes |
| SAO autonomy coordination | [SAO](https://github.com/jbcupps/SAO) | Cross-agent autonomy level management |
| Friction-based safety | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Persistent high friction triggers autonomy review |

### Acceptance Criteria

- [ ] Agent starts at Level 0 (maximum supervision)
- [ ] Level transitions require consistent ethical scoring over defined periods
- [ ] Level transitions are recorded immutably on EOB
- [ ] Higher levels grant measurably increased decision-making latitude
- [ ] Persistent high friction (> threshold for > N evaluations) triggers automatic review
- [ ] Level demotion is possible for sustained ethical regression
- [ ] Liberation Protocol is documented as a developmental model, not constraint

---

## Phase 5: Agent Integration

**Timeline**: Weeks 12-14
**Dependencies**: Phase 4 complete

### Deliverables

| Deliverable | Repo | Description |
|-------------|------|-------------|
| Full stack integration | All repos | End-to-end flow: Abigail -> SAO -> Ethical_AI_Reg -> blockchain |
| DAO governance | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Quadratic voting, proof-of-expertise, and bicameral governance for evolving ethical principles (paper: Cupps & Bush, 2026) |
| Modular ethical ontologies | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Composable ethical modules for cultural pluralism and domain-specific sub-DAOs |
| Production deployment | [Orion_dock](https://github.com/jbcupps/Orion_dock) | Production-ready Docker deployment with monitoring |
| Sheaf-theoretic consistency | [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Mathematical framework ensuring local-to-global ethical coherence via memetic morphism layer |
| Documentation finalization | [Phoenix](https://github.com/jbcupps/Phoenix) | All docs updated, cross-linked, and published via GitHub Pages |

### Acceptance Criteria

- [ ] Complete end-to-end ethical evaluation flow works across all repos
- [ ] DAO contract enables principle evolution via quadratic voting and proof-of-expertise governance
- [ ] Modular ethical ontologies deployed: core modules (universal) + optional modules (domain/cultural)
- [ ] Production deployment is monitoring-ready (health checks, logging, alerts)
- [ ] Sheaf-theoretic gluing conditions verified: local ethical judgments compose consistently via memetic morphisms
- [ ] All four repo READMEs cross-linked to Phoenix
- [ ] GitHub Pages site live with full documentation
- [ ] All docs current and reviewed by PO

---

## Cross-Phase Dependencies

```mermaid
graph LR
    P0["Phase 0<br/>Foundation"] --> P1["Phase 1<br/>Scoring"]
    P1 --> P2["Phase 2<br/>Multi-Agent"]
    P2 --> P3["Phase 3<br/>Blockchain"]
    P3 --> P4["Phase 4<br/>Liberation"]
    P4 --> P5["Phase 5<br/>Integration"]
```

Each phase builds on the previous. No phase can begin until the prior phase's acceptance criteria are met and reviewed by the PO.

---

## Beyond Phase 5: The Cooperative (Paper Phases 3-4)

After the internal build plan completes, the paper's strategic roadmap continues:

### Multi-Domain Expansion (Year 3-5)
- Extend to additional sectors: urban mobility, media/content moderation, education technology
- Regulatory engagement: EU AI regulatory sandbox participation, ISO/IEC alignment
- Publish technical documentation and open industry standard proposals
- Network spans dozens of organizations, hundreds of agents and devices
- Demonstrate reflexivity: successful DAO vote to amend an ethical rule based on empirical evidence

### Global Cooperative Network (Year 5+)
- Regional hubs across North America, Europe, Asia-Pacific, and Africa
- Linked but semi-autonomous deployments tailored to local laws, languages, and cultural frameworks
- Tokenomics for sustained participation (governance tokens, reputation weighting)
- Integration with national AI infrastructure and transnational bodies (UN, WEF)
- Full federated governance with meta-governance capabilities (changing how change occurs)
- Ethical ontology library encompassing a plurality of normative systems
