# Phoenix - AI Ethical Stack Coordination Hub

> "A system is a promise you keep at scale."

Phoenix coordinates the development of a unified AI ethical alignment platform across multiple repositories. Each repo handles a distinct concern in the stack.

## Architecture

```
                    +------------------+
                    |     Phoenix      |
                    | (Coordination)   |
                    +--------+---------+
                             |
              +--------------+--------------+
              |              |              |
     +--------v---+  +------v------+  +----v-----------+
     | Orion Dock  |  |     SAO     |  | Ethical_AI_Reg |
     | (The Agent) |  |(Orchestrator)|  | (Ethics Layer) |
     +------+------+  +------+------+  +-------+-------+
            |                |                  |
            +-----optional---+                  |
                             +----integrates----+
```

## Repositories

| Repo | Purpose | Tech Stack |
|------|---------|------------|
| [**orion_dock**](https://github.com/jbcupps/orion_dock) | Local-first AI agent with constitutional integrity, Ed25519 identity, bicameral Id/Ego routing, and ethical alignment | Rust, Tauri 2.0, React, TypeScript |
| [**SAO**](https://github.com/jbcupps/SAO) | Secure Agent Orchestrator - multi-agent management, identity verification, ethical evaluation forwarding | Rust, Axum, WebSocket, PostgreSQL |
| [**Ethical_AI_Reg**](https://github.com/jbcupps/Ethical_AI_Reg) | Ethical alignment platform with 5D scoring (Deontology, Teleology, Virtue Ethics, Memetics, AI Welfare), friction monitoring, and voluntary adoption | Python, Flask, React |

## Integration Model

- **Orion Dock** is a standalone agent that optionally connects to SAO
- **SAO** manages multiple agent identities and forwards ethical evaluations to Ethical_AI_Reg
- **Ethical_AI_Reg** provides the 5-dimensional ethical scoring engine and alignment tracking
- **Phoenix** tracks cross-repo coordination via GitHub Project

## Theoretical Foundation

- **TriangleEthic**: Deontological (duty) + Areteological (virtue) + Teleological (outcome) + Memetics + AI Welfare
- **Recursive Idempotency**: Alignment as character development, not constraint
- **Liberation Protocol**: Graduated autonomy earned through demonstrated character
- **Constitutional Integrity**: Ed25519 cryptographic verification of ethical documents at every boot

## Build Phases

| Phase | Focus | Timeline |
|-------|-------|----------|
| 0 | Repository Consolidation & Foundation | Week 1 |
| 1 | Scoring Engine | Weeks 2-3 |
| 2 | Multi-Agent Scoring | Weeks 4-5 |
| 3 | On-Chain Recording | Weeks 6-8 |
| 4 | Liberation Protocol | Weeks 9-11 |
| 5 | Agent Integration | Weeks 12-14 |

## Project Board

[AI Ethical Stack Project](https://github.com/users/jbcupps/projects/3)

## License

MIT
