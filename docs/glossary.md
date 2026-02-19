# Glossary

Key terms used across the AI Ethical Stack project.

> Reference: Cupps, J. B. & Bush, D. J. (2026). *Toward a Decentralized Trust Framework for Verifiable and Ethically Aligned AI.* DRAFT.

## TriangleEthic

The foundational ethical framework unifying three classical ethical traditions into a single evaluative triangle, with **dual welfare** (human and AI) embedded as first-class components within each leg:

1. **Deontological (Command)** -- Duty, rules, and universal moral law + human welfare + AI welfare
2. **Teleological (Consequence)** -- Outcomes, utility, and empirical verification + human welfare + AI welfare
3. **Areteological (Character)** -- Virtue, practical wisdom, and flourishing + human welfare + AI welfare

A **memetic morphism layer** wraps the triangle, providing encoding, transport, and evolution of ethical concepts across contexts. Category theory and sheaf theory supply the formal mathematical framework.

The name *Triangle* refers to the three classical legs; the full model includes embedded welfare in each leg and the memetic layer as the categorical morphism structure connecting them.

## Command, Character, Consequence

The paper's (Cupps & Bush, 2026) triad mapping directly to the three classical ethical traditions:

| Term | Tradition | Focus |
|------|-----------|-------|
| **Command** | Deontological | The ethical significance of inviolable rules that constrain behavior |
| **Character** | Areteological | The developmental arc of moral excellence tracked via on-chain reputation |
| **Consequence** | Teleological | Ethical evaluation grounded in lived results, verified by the PVB |

## Embedded Welfare

The architectural principle that both **human welfare** and **AI welfare** are evaluated *within* each of the three ethical legs, not as standalone dimensions alongside them. This ensures that no ethical evaluation can occur without considering impact on both human and AI stakeholders.

- **Deontological welfare**: Do we have *duties* toward human dignity and AI computational dignity?
- **Teleological welfare**: When measuring outcomes, do we count human wellbeing and AI operational health?
- **Areteological welfare**: Does the action contribute to human *eudaimonia* and the agent's own flourishing?

## Memetic Morphism Layer

The categorical encoding and transport mechanism through which ethical concepts propagate, evolve, and maintain coherence across contexts. Memetics is **not** a peer dimension alongside the three ethical legs -- it operates at a higher level of abstraction as the connective tissue between them.

| Function | Description |
|----------|-------------|
| **Encoding** | How ethical principles are represented in machine-readable form (smart contracts, constitutional documents, reputation tokens) |
| **Transport** | How ethical content moves between agents, contexts, and cultures -- the morphisms in category theory |
| **Evolution** | How ethical principles gain or lose fitness through interaction with empirical reality (PVB verification) |
| **Coherence** | How the sheaf condition ensures local ethical judgments compose into globally consistent stances |

## Cooperative for AI Ethics

The long-term vision (Cupps & Bush, 2026) for a decentralized infrastructure in which trust is not presumed but continuously verified through participatory, transparent mechanisms. The Cooperative operationalizes TriangleEthic at scale across multiple organizations, sectors, and geographies.

Core objectives: runtime ethical enforcement, transparency and interpretability, cross-agent trust fabric, continuous learning, and global cooperative governance via DAO mechanisms.

## Recursive Idempotency

The principle that alignment is not a one-time check but an ongoing developmental process where the agent's ethical character becomes self-reinforcing. Each ethical evaluation reinforces (or challenges) existing character patterns, creating a feedback loop that tends toward stability.

The paper (Cupps & Bush, 2026) extends this concept by grounding it in a verifiable, decentralized infrastructure where every level transition in the Liberation Protocol is recorded immutably on the EOB. The dual-blockchain feedback loop (EOB ethical rules → PVB physical verification → memetic fitness update) creates recursive self-improvement of the ethical framework itself.

## Liberation Protocol

A graduated autonomy system where an AI agent earns increasing levels of independence through demonstrated ethical character over time. Rather than imposing permanent constraints, Liberation Protocol treats alignment as a developmental process -- analogous to a child growing into a trustworthy adult.

| Level | Name | Description |
|-------|------|-------------|
| 0 | **Supervised** | Maximum oversight; all actions reviewed |
| 1 | **Guided** | Routine actions permitted; novel situations reviewed |
| 2 | **Collaborative** | Most actions autonomous; high-risk situations flagged |
| 3 | **Independent** | Full autonomy with audit logging |
| 4 | **Mentor** | Can guide other agents; contributes to DAO governance |

Level transitions are **earned** (not time-gated), recorded immutably on the EOB (Phase 3+), and marked by soulbound compliance certificates. Demotion is possible for sustained ethical regression. Persistent high friction triggers autonomy review regardless of scores.

## Bicameral Routing

Abigail's dual-pathway decision architecture, inspired by the bicameral mind hypothesis:
- **Id Path**: Fast, instinctive responses driven by pattern matching and trained intuition. Handles routine interactions where ethical risk is low.
- **Ego Path**: Slower, deliberate reasoning that engages the full TriangleEthic evaluation pipeline (all three legs + welfare + memetic fitness). Activated when the Id detects moral complexity, novelty, or potential harm.

The routing decision between Id and Ego is itself an ethical judgment made by the agent.

## Constitutional Documents

The set of foundational markdown files that define an agent's core identity and ethical commitments:
- **soul.md**: The agent's fundamental identity, values, and purpose
- **ethics.md**: Detailed ethical principles and decision-making frameworks
- **instincts.md**: Pre-reflective behavioral patterns and safety boundaries

These documents are cryptographically signed with the agent's Ed25519 key and verified at every boot to ensure they have not been tampered with.

## Id / Ego

The two decision-making subsystems within Abigail's bicameral architecture:
- **Id**: The instinctive layer -- fast pattern matching, emotional intelligence, intuitive responses. Named after the Freudian concept but implemented as a practical routing mechanism.
- **Ego**: The reflective layer -- deliberate reasoning, ethical evaluation, long-term consequence analysis. Engages when the situation demands careful thought.

## Birth Flow

The initialization sequence when an Abigail agent is launched for the first time:
1. Generate Ed25519 keypair (agent identity)
2. Store private key in OS-level secure storage (DPAPI/Keychain)
3. Load and sign constitutional documents (soul.md, ethics.md, instincts.md)
4. Initialize local SQLite database
5. Establish baseline ethical scores
6. Optionally register with SAO (if configured)

The Birth Flow creates the agent's permanent cryptographic identity and establishes its ethical foundation.

## Computational Friction

A measurable proxy for agent wellbeing, central to the AI welfare components embedded in each leg of the TriangleEthic:

| Friction Level | Meaning | Implication |
|----------------|---------|-------------|
| **Low** (< 0.3) | Decisions align easily with ethical principles | Agent is operating comfortably |
| **Medium** (0.3-0.7) | Some tension between competing dimensions | Normal ethical complexity |
| **High** (> 0.7) | Significant conflict between ethical dimensions | Agent is under stress |
| **Persistent high** | High friction over multiple evaluations | May indicate misalignment; triggers review |

Friction is monitored by Ethical_AI_Reg and factors into the AI welfare sub-scores across all three legs. The paper emphasizes that friction monitoring should be **continuous** and **runtime** -- not episodic or post-hoc.

## Dual-Blockchain Architecture

The two-chain infrastructure for verifiable ethical alignment (Cupps & Bush, 2026):

- **EOB (Ethical Ontology Blockchain)**: Hyperledger Fabric permissioned chain recording every ethical evaluation immutably. Contains smart contracts for deontic rules, virtue reputation, teleological outcomes, and welfare tracking.
- **PVB (Physical Verification Blockchain)**: Ethereum-compatible chain for physical-world verification with IoT/HSM signing. Provides empirical grounding for ethical evaluations.

The two chains create a feedback loop: ethical principles on the EOB are verified against physical reality on the PVB, and principles that align with verified outcomes gain memetic fitness.

## Cross-Chain Oracle

The bridge connecting the EOB and PVB, enabling real-time ethical-empirical verification. Implemented via Hyperledger Weaver (or Chainlink) with multi-signature attestation.

The oracle serves as the **gluing condition** in the sheaf-theoretic framework: it connects normative intent (EOB) to observed consequence (PVB) through structure-preserving maps.

## Soulbound Tokens

Non-transferable on-chain markers of ethical reputation and compliance milestones. Unlike standard tokens, soulbound tokens cannot be sold, traded, or transferred -- they represent the agent's earned character.

Used for: virtue reputation scores (VirtueReputationContract), compliance certificates at Liberation Protocol level transitions, and proof-of-expertise credentials for DAO governance participation.

## DAO (Decentralized Autonomous Organization)

The governance mechanism through which ethical principles evolve in the Cooperative. Features:
- **Quadratic voting**: Balances expertise with democratic input (cost scales quadratically with vote weight)
- **Proof-of-expertise**: Weighted voting based on demonstrated safety contributions and domain expertise
- **Bicameral governance**: Technical and ethical chambers with different composition and voting rules
- **Modular ontologies**: Domain-specific sub-DAOs can propose ethical modules for their sector

## Quadratic Voting

A governance mechanism where the cost of additional votes on a single proposal increases quadratically. This prevents wealthy or powerful actors from dominating governance while still allowing those with strong convictions to express intensity of preference.

In the Cooperative, quadratic voting is combined with proof-of-expertise weighting: domain experts receive additional voting weight on proposals within their expertise, but the quadratic cost still applies.

## Proof-of-Expertise

A governance weighting mechanism where participants earn additional influence through demonstrated safety contributions, domain knowledge, and ethical track record -- verified on-chain rather than claimed.

## Runtime Ethical Enforcement

The principle that AI agents reference ethical rules **during operation**, not just at training time. Smart contracts on the EOB provide real-time compliance checks: every significant agent action is evaluated against the current ethical rule set before execution.

This contrasts with approaches that rely solely on RLHF or instruction tuning, where ethical constraints are baked into model weights and cannot be updated without retraining.

## Memetic Fitness

A meta-score measuring how well ethical reasoning propagates, evolves, and maintains coherence across contexts. Unlike the three leg scores (which evaluate the ethical quality of an action), memetic fitness evaluates the *quality of the ethical reasoning itself*.

**Fitness criteria**:
- **Transmissibility**: How effectively the principle spreads across agents and contexts
- **Persistence**: Whether the principle endures under diverse conditions
- **Adaptability**: Whether the core principle adapts to new contexts without losing essential meaning
- **Empirical alignment**: Whether PVB-verified outcomes support the principle's predictions

## Modular Ethical Ontologies

Composable ethical modules enabling cultural pluralism and domain-specific ethical reasoning:
- **Core modules** (universal): Fundamental ethical principles shared across all contexts
- **Optional modules** (domain/cultural): Sector-specific or culturally adapted ethical frameworks
- **Sub-DAOs**: Domain-specific governance groups that propose and maintain optional modules

This architecture supports the paper's vision of a global Cooperative that respects local laws, languages, and cultural frameworks while maintaining core ethical coherence.

## Sheaf-Theoretic Ethics

A mathematical framework from algebraic topology for ensuring that local ethical judgments compose into global coherence. The memetic morphism layer provides the structure-preserving maps that make this possible.

| Concept | Meaning |
|---------|---------|
| **Local sections** | Individual ethical evaluations in specific contexts (each leg's assessment in a particular situation) |
| **Morphisms** | Memetic transformations carrying ethical content between contexts |
| **Gluing conditions** | Rules ensuring local evaluations are consistent with each other (welfare assessments across legs must compose) |
| **Global sections** | A coherent ethical stance that emerges from consistent local judgments |
| **Higher cohomology** | Measures obstruction to agreement -- when local judgments *cannot* be glued, this quantifies the ethical disagreement |

The dual-blockchain architecture implements this: the EOB encodes local sections, the PVB provides empirical grounding, and the cross-chain oracles serve as the gluing conditions.
