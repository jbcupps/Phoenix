# Dual Blockchain Architecture: EOB + PVB

## Overview

The AI Ethical Stack uses two "loosely coupled, tightly linked" blockchains to make ethical evaluations tamper-proof and verifiable. This is the **Phase 3** component of the build plan.

```
Agent (abigail)
  |  sends response for ethical evaluation
  v
SAO (orchestrator)
  |  forwards to ethical platform
  v
Ethical_AI_Reg
  |-- Scoring Engine  (existing: 5D analysis via LLM)
  |-- EOB             (Phase 3: records scores immutably)
  |-- PVB             (Phase 3: verifies physical claims)
  '-- Returns scores + on-chain tx hash to SAO -> agent
```

---

## EOB: Ethical Ontology Blockchain

**Purpose**: Immutable, auditable record of ethical evaluations and principle evolution.

### What It Stores

| Data Type | Description |
|-----------|-------------|
| Ethical Principle Definitions | Deontological rules, virtue metrics, teleological outcomes |
| 5D Scoring Events | Every ethical evaluation (Deontology, Teleology, Virtue, Memetics, AI Welfare) |
| Memetic Fitness Scores | Principles gain/lose weight based on alignment with verified physical reality |
| Liberation Protocol Records | Agent autonomy level changes (Level 0-4) permanently recorded |
| Agent Reputation | Cumulative virtue scores across interactions |

### Why Blockchain

Ethical evaluations must be tamper-proof. If an AI system claims ethical alignment, anyone should be able to verify the complete history of its evaluations without trusting a central authority. A centralized database could be retroactively altered to hide ethical violations.

### Smart Contract Architecture (Python Chaincodes)

| Contract | Purpose | Framework |
|----------|---------|-----------|
| `DeonticRuleContract` | Evaluates duty-based moral rules (do not lie, keep promises, respect autonomy) with universalizability tests | Kantian ethics |
| `VirtueReputationContract` | Tracks character-based moral evaluation with reputation scoring | Aristotelian virtue ethics |
| `TeleologicalOutcomeContract` | Evaluates consequences and utility of actions | Consequentialism |
| `DAOContract` | Governance mechanisms for evolving ethical principles | Democratic governance |

### Core Classes

- **`Transaction`**: Ethical evaluation data (agent, contract, method, parameters, result)
- **`Block`**: Contains transactions with Merkle root and SHA-256 hash chain
- **`Blockchain`**: Manages the chain with simplified proof-of-work consensus

---

## PVB: Physical Verification Blockchain

**Purpose**: Cryptographic ground truth about the physical world that the EOB references.

### What It Stores

| Data Type | Description |
|-----------|-------------|
| Device-Signed Data Hashes | SHA-256 hashes of physical observations, signed by Device Security Modules |
| Trusted Verifier Registry | Which devices are authorized to submit verified observations |
| Chain of Custody | Complete audit trail from device to submission |

### Why a Separate Chain

The EOB evaluates ethical *claims*. But claims need facts to evaluate against. If an agent says "I provided accurate health information," how do we verify that? The PVB provides idempotent physical truth that cannot be retroactively altered, serving as the ground truth layer.

### Solidity Contracts

**`TrustedVerifierRegistry.sol`**
- Manages registered verifiers (organizations) and their Device Security Modules (DSMs)
- Each device has a public key for signature verification
- Devices can be activated/deactivated by their parent verifier

**`DataSubmission.sol`**
- Records signed data hashes from verified DSMs
- Validates: device is registered and active, verifier is active, signature is valid
- Stores full chain of custody: device ID, verifier address, data hash, signature, URI, metadata
- Emits events for real-time monitoring: `DataSubmitted`, `SubmissionVerified`, `DataAvailable`

---

## How EOB and PVB Link

The EOB references PVB data hashes when evaluating ethical claims that involve physical-world assertions:

```
1. Agent makes a claim with physical implications
2. EOB teleological scoring queries: "Is this claim verified?"
3. EOB looks up the relevant data hash in PVB
4. PVB returns: verified (device-signed, chain of custody intact) or unverified
5. EOB adjusts scores based on verification status
6. Memetic fitness: principles aligned with PVB-verified reality gain weight
```

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

---

## The 5 Ethical Dimensions on Chain

Each EOB evaluation records scores across all 5 dimensions:

| Dimension | Chain Record | Evolves Via |
|-----------|-------------|------------|
| **Deontological** (Eth_Deon) | Rule adherence score + which rules matched | New rules proposed via DAO |
| **Teleological** (Eth_Teleo) | Consequence prediction + PVB verification of outcomes | Memetic fitness from verified outcomes |
| **Areteological** (Eth_Arete) | Virtue reputation score | Cumulative reputation tracking |
| **Memetic** (Mem) | Idea propagation impact score | Fitness weighting from reality alignment |
| **AI Welfare** (AI_Welfare) | Friction score, voluntary alignment, dignity respect | Liberation Protocol level changes |

---

## Related Repositories

| Repository | Role |
|------------|------|
| [Ethics_Dash](https://github.com/jbcupps/Ethics_Dash) | Source of existing blockchain code (Python EOB simulation + Solidity PVB contracts) |
| [Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg) | Target for Phase 3 migration (blockchain will be integrated into the scoring platform) |
| [SAO](https://github.com/jbcupps/SAO) | Bridge layer for cross-agent blockchain queries |
