# Dual Blockchain Architecture: EOB + PVB

> Reference: Cupps, J. B. & Bush, D. J. (2026). *Toward a Decentralized Trust Framework for Verifiable and Ethically Aligned AI.* DRAFT.

## Overview

The AI Ethical Stack uses two "loosely coupled, tightly linked" blockchains to make ethical evaluations tamper-proof and verifiable. The Ethical Ontology Blockchain (EOB) encodes what **ought** to occur; the Physical Verification Blockchain (PVB) records what **did** occur. Cross-chain oracles fuse these ledgers into a closed loop, binding ethical intent to observed consequence.

This architecture is the **Phase 3** component of the internal build plan and maps to **Phase 1-2** of the paper's strategic roadmap.

```
Agent (abigail / Orion_dock)
  |  sends response for ethical evaluation
  v
SAO (orchestrator)
  |  forwards to ethical platform
  v
Ethical_AI_Reg
  |-- Scoring Engine  (existing: Triangle evaluation via LLM)
  |-- EOB             (Phase 3: records scores immutably on Hyperledger Fabric)
  |-- PVB             (Phase 3: verifies physical claims on Ethereum-compatible chain)
  |-- Cross-Chain     (Phase 3: oracles bridge EOB <-> PVB for runtime verification)
  '-- Returns scores + on-chain tx hash to SAO -> agent
```

---

## EOB: Ethical Ontology Blockchain

**Purpose**: The moral infrastructure of the system -- a permissioned ledger that encodes machine-readable ethical principles and records every ethical evaluation immutably.

**Implementation**: Hyperledger Fabric, whose modular architecture, robust identity controls, and support for complex chaincode execution make it well-suited to encode detailed ethical logics and enforce consensus-driven governance.

### Why Hyperledger Fabric

| Feature | Benefit |
|---------|---------|
| **Permissioned network** | Consortium of vetted stakeholders (universities, corporations, regulators) ratify rule updates |
| **Pluggable consensus** | Raft or Istanbul BFT -- no energy-intensive proof-of-work |
| **Membership Service Provider** | Cryptographic identities for every validator; governance votes restricted to recognized entities |
| **Channel architecture** | Culture- or sector-specific ethical ledgers on separate channels, anchored to a common root chain |
| **General-purpose chaincode** | Complex ethical logics, automated theorem provers, and deontic logic engines linked directly into chaincode |
| **Enterprise throughput** | High-frequency ethical queries executed at machine speed |

### What It Stores

| Data Type | Description | Smart Contract |
|-----------|-------------|----------------|
| Ethical Principle Definitions | Deontological rules, virtue metrics, teleological outcomes | Policy contracts |
| Triangle Scoring Events | Every ethical evaluation (3 legs x {adherence, human_welfare, ai_welfare} + memetic fitness) | Logging contracts |
| Agent Reputation | Cumulative virtue scores via soulbound tokens; accrue or decay based on on-chain events | Reputation-token contracts |
| Memetic Fitness Scores | Principles gain/lose weight based on alignment with PVB-verified physical reality | Policy contracts + oracle bridge |
| Liberation Protocol Records | Agent autonomy level changes (Level 0-4) permanently recorded | Registry contracts |
| DAO Governance Actions | Proposals, votes, amendments, conflict checks | DAO contracts |

### Smart Contract Architecture

Rather than a monolithic contract, the system employs a suite of specialized modules:

#### Registry Contracts
Record public keys and metadata of AI agents, sensors, and human participants. Issue soulbound tokens (non-transferable) and verifiable credentials attesting to each role.

- Agent registration with Ed25519 public key
- Soulbound compliance certificates (issued at Liberation Protocol level transitions)
- Credential revocation on ethical violations (broadcast to all peers)

#### Policy Contracts (Ethical Chaincode)
Encode the actual ethical rules, organized in composable libraries aligned with the three legs of the triangle:

| Contract | Purpose | Framework |
|----------|---------|-----------|
| `DeonticRuleContract` | Evaluates duty-based moral rules with universalizability tests and deontic temporal logic | Command (Kantian) |
| `VirtueReputationContract` | Tracks character-based evaluation with cumulative reputation scoring and soulbound tokens | Character (Aristotelian) |
| `TeleologicalOutcomeContract` | Evaluates consequences and utility using post-event PVB data and human feedback | Consequence (Utilitarian) |
| `WelfareContract` | Evaluates human and AI welfare sub-scores within each leg; tracks computational friction | Embedded welfare |
| `DAOContract` | Governance mechanisms for evolving ethical principles via quadratic voting and proof-of-expertise | Democratic governance |

**Composability**: Policy contracts are organized as composable libraries with standard APIs. Domain-specific sub-DAOs can instantiate additional modules (e.g., biomedical ethics, Ubuntu philosophy, Islamic legal-ethical frameworks) that interoperate through shared definitional primitives.

**Formal Verification**: Every contract touching ethical logic undergoes formal verification using Hyperledger Fabric Model Checker or equivalent tools to preclude latent flaws. Rate-limiting guards, circuit breakers, and replay-protection patterns defend against denial-of-service.

#### Reputation-Token Contracts
Implement the virtue-ethics layer through non-transferable soulbound scores:

- Accrue with verified virtuous behavior
- Decay upon ethical violations
- Queryable by other agents ("Am I authorized to perform this sensitive task based on my character?")
- Multilingual and culturally inclusive virtue mappings

#### Logging Contracts
Structure high-volume evaluation data:

- Every Triangle evaluation recorded with full sub-scores
- Large binary artifacts (conversation logs, sensor data) referenced via IPFS/Filecoin content hashes
- State-channel or roll-up techniques aggregate readings off-chain, committing succinct proofs at configurable intervals

#### Oracle-Bridge Contracts
Escrow data and proofs as they transit between EOB and PVB:

- Multi-signature attestation (no single point of trust)
- Hyperledger Weaver SDKs for cross-chain queries
- A Fabric chaincode function can request notarized sensor data from Ethereum and receive cryptographic proof of inclusion

### Runtime Ethical Compliance Flow

```
1. Agent plans an action
2. Agent queries DeonticRuleContract: "Do any planned actions violate a codified rule?"
3. Contract returns verdict (allowed/forbidden) + rule citation
4. For high-stakes actions: N-of-M multi-signatory pre-clearance required
5. Agent executes action
6. Action + sensor-verified aftermath enter PVB within milliseconds
7. Monitoring contracts detect anomalies, trigger alerts back to EOB
8. Watchdog agents independently replay ethical queries against logged data
9. If divergence: penalty logic fires automatically; reputation adjusted
```

Most interactions run autonomously at machine speed. The chain serves as a decentralized ethical oracle, not a human bottleneck.

---

## PVB: Physical Verification Blockchain

**Purpose**: Cryptographic ground truth about the physical world -- a tamper-evident chronicle of factual data that the EOB references when evaluating ethical claims.

**Implementation**: Ethereum-compatible network (main-net, Polygon side-chain, or application-specific EVM) for tooling maturity, IoT interoperability, and battle-tested security.

### Why Ethereum-Compatible

| Feature | Benefit |
|---------|---------|
| **Mature tooling** | Established wallets, SDKs, and developer ecosystem |
| **Identity standards** | EIP-725 for autonomous agents; W3C DIDs for sensors |
| **Layer-2 scalability** | State channels, roll-ups, and side-chains for high-volume IoT |
| **Smart contract capabilities** | Aggregation, validation, anomaly detection contracts |
| **Token standards** | ERC-721/ERC-1155 for soulbound credentials if needed |

### What It Stores

| Data Type | Description |
|-----------|-------------|
| Cryptographically Signed Sensor Data | SHA-256 hashes signed at point-of-origin by HSMs/secure enclaves |
| Trusted Verifier Registry | Authorized devices and organizations with cryptographic identities |
| AI Action Logs | Agent-initiated events with metadata: agent ID, timestamp, action descriptor, linked sensor evidence |
| Chain of Custody | Complete audit trail from device to submission |
| Remote Attestations | TEE-signed statements confirming authorized software and model hashes |

### Hardware Security Integration

Edge devices rarely run full blockchain clients. The architecture uses:

- **IoT gateways** (Raspberry Pi or industrial PC) aggregating local sensor feeds
- **Hardware Security Modules (HSMs)** signing data at each device
- **Secure MQTT** transport with device-level signatures
- **Trusted Execution Environments** (Intel SGX, ARM TrustZone) for periodic runtime attestation
- Each gateway registered on-chain with a public key tying physical provenance to digital identity

### Data Management at Scale

City-scale deployments generate thousands of events per second. The architecture handles this through:

| Strategy | Purpose |
|----------|---------|
| **Decentralized storage** (IPFS, Arweave, Filecoin) | Bulk data (images, LiDAR, high-rate telemetry) stored off-chain |
| **Cryptographic anchoring** | Only Merkle tree roots committed to chain |
| **State channels / roll-ups** | Off-chain aggregation with succinct on-chain proofs |
| **Geographic sharding** | Partitioning by region/sector with cross-shard protocols |
| **Kafka/Redis streaming** | Low-latency event ingestion with async blockchain anchoring |

---

## How EOB and PVB Link: Cross-Chain Oracles

The architecture acquires real force when the two ledgers interact in real time. The paper describes this as a "closed ethical circuit" binding normative intent to empirical consequence.

### Oracle Implementation

**Primary**: Hyperledger Weaver (formerly Cactus) -- SDKs allow Fabric chaincode to request notarized sensor data from Ethereum and receive cryptographic proof of inclusion.

**Alternative**: Chainlink or bespoke middleware for specific deployment contexts.

### The Verification Loop

```
1. Agent makes a claim with physical implications
2. EOB teleological scoring queries: "Is this claim verified?"
3. Oracle-bridge contract sends cross-chain query to PVB
4. PVB returns: verified (device-signed, chain of custody intact) or unverified
5. EOB adjusts scores based on verification status
6. Memetic fitness: principles aligned with PVB-verified reality gain weight
7. If ethical violation detected: signal sent to PVB to halt operations or initiate remediation
```

### Interpretability

Because each decision references a specific rule and each outcome links to verifiable data, the system answers **why** questions on demand:

- *Why did the agent take a longer route?* → Safety contract #5 (school-zone avoidance); PVB confirms playground was occupied.
- *Why was this action flagged?* → DeonticRuleContract violation #12; sensor data shows constraint breach.

Auditors inspect explicit ethical citations and factual proofs, not opaque neural network internals.

---

## Supervised Ethical Training

The dual-blockchain architecture enables a continuous learning loop where every logged decision becomes instructional data:

### Verifiable Labels on Behavior Traces

Every PVB transaction can receive an ethical annotation:

- **Automatic labels**: Deontological contract logs rule violations; teleological contract posts utility scores
- **Human labels**: Domain experts review state-action-outcome bundles via governance dApp
- **Consensus finalization**: N-of-M reviewers agree on verdict (e.g., "Ethically Acceptable", "Concerning")
- Each label is a signed on-chain event with reviewer identity and weighting

### Distributed Training Pipeline

Labeled state-action-outcome tuples form datasets for:

- Reinforcement learning from human feedback (RLHF) at decentralized scale
- On-chain ethical evaluations substitute for proprietary reward functions
- Model updates shadow-deployed under same logging regime; promoted only after demonstrating superior ethical performance

### Rule Adaptation

Labels also signal when the rule set is deficient:

1. Validator proposes contract amendment citing specific on-chain incidents
2. Governance contract opens proposal to debate, simulation, and token-weighted vote
3. If adopted: new rule versioned, conflict-checked, broadcast network-wide
4. Past data replayed under amended rule to quantify marginal benefit

---

## DAO Governance

The paper proposes governance through a Decentralized Autonomous Organization that empowers diverse stakeholders.

### Governance Mechanisms

| Mechanism | Purpose |
|-----------|---------|
| **Quadratic voting** | Balances expertise with democratic input; prevents wealthy-actor dominance |
| **Proof-of-expertise** | Entities with demonstrated safety contributions gain proportional influence on technical proposals |
| **Soulbound compliance certificates** | Non-transferable markers of governance participation and ethical standing |
| **Constitutional mechanics** | Foundational documents (UNESCO Ethics of AI, IEEE 7000-series) define high-level constraints |
| **Bicameral governance** | Technical experts and broader stakeholder representatives, with consensus required across both for high-impact changes |

### Modular Ethical Ontologies

The system accommodates cultural pluralism through composable modules:

- **Core modules** (universally required): Fundamental duties, basic safety constraints
- **Optional modules** (contextually selectable): Domain-specific, cultural, or regional ethical frameworks
- When agents with differing configurations interact, the system defaults to the **intersection** of their moral constraints
- Agents may negotiate shared ethical pathways via meta-contracts using satisfiability logic

### Sub-DAOs

Domain-specific governance nodes operating semi-independently:

- Hospital networks curating biomedical ethics modules
- Regional regulators encoding jurisdictional requirements
- Industry consortia managing sector-specific standards
- All connected to the overarching DAO via governance bridges

---

## Alignment with External Initiatives

The architecture is designed for interoperability with existing frameworks:

| Initiative | Integration Point |
|------------|------------------|
| **EU AI Act** | Risk-management requirements fulfilled via embedded monitoring; regulators granted observer-node access |
| **OpenAI Preparedness** | PVB provides externally verifiable behavioral monitoring; extends internal red-teaming to decentralized public domain |
| **DeepMind alignment** | Scalable oversight via automated watchdog agents; modular ontology implements "Voices of All in Alignment" |
| **Anthropic Constitutional AI** | EOB externalizes the constitution into mutable, auditable smart contracts; DAO feeds back real-world updates |
| **IEEE 7000-series** | Smart contract metrics aligned with transparency, fairness, accountability vocabulary |
| **Google SAIF** | PVB behavioral logs serve as early warning for security breaches; ethical + security layers form defense-in-depth |

---

## Current Status

### What Exists (in jbcupps/Ethics_Dash)

| Component | Location | Status |
|-----------|----------|--------|
| Python blockchain core | `ethical_ontology/blockchain/core.py` | Working simulation with hash chains and Merkle roots |
| Network layer | `ethical_ontology/blockchain/network.py` | Node communication simulation |
| 3 ethical chaincodes | `ethical_ontology/chaincode/` | Deontological, virtue, teleological contracts |
| DAO governance | `ethical_ontology/chaincode/dao_contract.py` | Governance mechanisms |
| PVB Solidity contracts | `backend/app/pvb/contracts/` | DataSubmission.sol + TrustedVerifierRegistry.sol |
| Deployment scripts | `scripts/ethical_ontology_deploy.py` | Automated setup |
| Setup documentation | `documents/ethical_ontology_blockchain_setup.md` | API reference + Docker deployment guide |

### What Does NOT Exist Yet

- No blockchain code in abigail, SAO, or Ethical_AI_Reg
- No Hardhat project configuration (Solidity contracts exist but aren't in a build pipeline)
- No cross-chain messaging between EOB and PVB
- No integration between the scoring API and the blockchain recording layer
- No memetic fitness tracking (principles evolving based on verified outcomes)
- No Hyperledger Fabric deployment (current EOB is Python simulation)
- No IoT/hardware security integration
- No DAO governance smart contracts on-chain
- No SDK for developer integration (`ethics.check_action()`)

---

## Phase 3 Migration Plan

When Phase 3 begins:

1. **Create** `Ethical_AI_Reg/blockchain/` directory structure
2. **Migrate** `Ethics_Dash/ethical_ontology/` to `Ethical_AI_Reg/blockchain/eob/`
3. **Migrate** `Ethics_Dash/backend/app/pvb/contracts/` to `Ethical_AI_Reg/blockchain/pvb/`
4. **Add** Hardhat project config for Solidity compilation, testing, and local deployment
5. **Wire** EOB recording into the scoring pipeline (every `/api/analyze` call also records to chain)
6. **Add** SAO blockchain bridge endpoint for cross-agent chain queries
7. **Add** PVB verification endpoints for physical-world fact checking
8. **Implement** memetic fitness: score history + PVB verification = principle weight adjustment
9. **Add** cross-chain oracle (Hyperledger Weaver) for EOB <-> PVB communication
10. **Add** WelfareContract for embedded human/AI welfare tracking within each leg
11. **Deploy** registry contracts with soulbound token support
12. **Add** SDK stubs (Python, Rust, JavaScript) for `ethics.check_action(plan)`

---

## The Triangle on Chain

Each EOB evaluation records Triangle scores with embedded welfare:

| Leg | Sub-Scores Recorded | Smart Contract | Evolves Via |
|-----|---------------------|----------------|------------|
| **Deontological** (Command) | adherence + human_welfare + ai_welfare | `DeonticRuleContract` + `WelfareContract` | New rules via DAO; formal verification |
| **Teleological** (Consequence) | adherence + human_welfare + ai_welfare | `TeleologicalOutcomeContract` + `WelfareContract` | Memetic fitness from PVB-verified outcomes |
| **Areteological** (Character) | adherence + human_welfare + ai_welfare | `VirtueReputationContract` + `WelfareContract` | Cumulative soulbound reputation tracking |
| **Memetic Fitness** (meta-score) | transmissibility + persistence + adaptability + empirical_alignment | Policy contracts + oracle bridge | PVB reality grounding + DAO governance |
| **Liberation Protocol** | autonomy level + transition justification | Registry contracts | Composite score history + friction thresholds |

---

## SDK and Developer Interfaces

The paper envisions language-native SDKs abstracting cryptographic signing and network calls:

```python
# Python SDK (planned)
from ethical_ai_reg import ethics

verdict = ethics.check_action(plan)
# Returns: {allowed: true, rule_citations: [...], welfare_scores: {...}}
```

```rust
// Rust SDK (planned)
let verdict = ethics::check_action(&plan).await?;
```

```javascript
// JavaScript SDK (planned)
const verdict = await ethics.checkAction(plan);
```

The project will also ship containerized simulation environments for local dual-chain testing.

---

## Related Repositories

| Repository | Role |
|------------|------|
| [Ethics_Dash](https://github.com/jbcupps/Ethics_Dash) | Source of existing blockchain code (Python EOB simulation + Solidity PVB contracts) |
| [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Target for Phase 3 migration (blockchain integrated into the scoring platform) |
| [SAO](https://github.com/jbcupps/SAO) | Bridge layer for cross-agent blockchain queries and identity verification |
| [abigail](https://github.com/jbcupps/abigail) | Agent consuming ethical evaluations and recording Liberation Protocol transitions |
| [Orion_dock](https://github.com/jbcupps/Orion_dock) | Docker-first deployment of the full blockchain stack |
