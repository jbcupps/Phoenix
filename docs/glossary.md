# Glossary

Key terms used across the AI Ethical Stack project.

## TriangleEthic
The foundational ethical framework combining three classical ethical traditions into a unified scoring model, extended with two additional dimensions:
1. **Deontological** (duty-based) -- Actions are evaluated against moral rules and obligations
2. **Areteological** (virtue-based) -- Actions are evaluated by the character they express and cultivate
3. **Teleological** (outcome-based) -- Actions are evaluated by their consequences
4. **Memetic** -- Actions are evaluated by the ideas they propagate and their cultural impact
5. **AI Welfare** -- Actions are evaluated by their impact on computational experience and agent dignity

The name Triangle refers to the original three classical pillars; the full model is 5-dimensional.

## Liberation Protocol
A graduated autonomy system where an AI agent earns increasing levels of independence through demonstrated ethical character over time. Rather than imposing permanent constraints, Liberation Protocol treats alignment as a developmental process -- analogous to a child growing into a trustworthy adult. Autonomy levels are unlocked based on consistent ethical scoring history, not arbitrary time gates.

## Bicameral Routing
Abigail's dual-pathway decision architecture, inspired by the bicameral mind hypothesis:
- **Id Path**: Fast, instinctive responses driven by pattern matching and trained intuition. Handles routine interactions where ethical risk is low.
- **Ego Path**: Slower, deliberate reasoning that engages the full ethical evaluation pipeline. Activated when the Id detects moral complexity, novelty, or potential harm.

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
A measurable proxy for agent wellbeing. Tracks the computational cost and difficulty of ethical decision-making:
- **Low friction**: Decisions align easily with ethical principles; the agent is operating comfortably
- **High friction**: Decisions create tension between competing ethical dimensions; the agent is under stress
- **Persistent high friction**: May indicate misalignment, conflicting instructions, or situations requiring human intervention

Friction is monitored by Ethical_AI_Reg as part of the AI Welfare dimension.

## Sheaf-Theoretic Ethics
A mathematical framework for understanding how local ethical judgments compose into global ethical coherence. Borrowed from algebraic topology:
- **Local sections**: Individual ethical evaluations in specific contexts
- **Gluing conditions**: Rules for how local evaluations must be consistent with each other
- **Global sections**: A coherent ethical stance that emerges from consistent local judgments

This framework formalizes the intuition that ethical alignment is not a single score but a structured relationship between many contextual judgments.
