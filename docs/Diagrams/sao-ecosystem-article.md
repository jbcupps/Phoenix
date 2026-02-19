# The Missing Layer: When Your AI Agent Needs a Badge

*Part of my ongoing series on AI experiments. The last article covered Orion Dock — what happens when you take agent identity seriously. This one's about what happens when you take it to work.*

---

Here's a question I've been sitting with: who issues the badge?

Not metaphorically. If an AI agent is going to operate inside an enterprise — reading your data, calling your APIs, acting on behalf of your employees — someone has to vouch for it. Someone has to say "this agent is authorized to be here, these are its permissions, and here's the cryptographic proof."

Right now, the answer at most companies is "whoever has the API key." Which is the same answer we gave for human access in 2005, and it worked out about as well as you'd expect.

I've been building three things that I think are pieces of the same answer. It took me embarrassingly long to realize they were connected.

---

## The Shape of the Problem

**[ILLUSTRATION: Ecosystem Overview]**

Think about the identity spectrum for a second. At one end, you've got a personal AI assistant running on your laptop. It knows your preferences, your writing style, maybe your calendar. Its identity is simple — it's yours.

At the other end, you've got a fleet of containerized agents running in production. They're processing claims, analyzing logs, summarizing reports. They need to authenticate against corporate services, respect RBAC boundaries, and leave audit trails. Their identity has to come from somewhere that your CISO will accept.

The gap between those two points is enormous. And nobody's building the bridge.

So I accidentally built three pieces of it.

---

## Abigail: The Personal Agent

Abigail is where this started — a personal AI agent running as a Tauri desktop app on my local machine. She's named after Abigail Normal from Young Frankenstein, because she's assembled from parts and finds that amusing rather than distressing.

Abigail runs a local LLM through Ollama for fast responses and routes to cloud models when she needs to think harder. Her identity lives on my machine — Ed25519 keys stored in a DPAPI-encrypted vault on Windows. She has a constitution that defines her values and boundaries. She has memory that crystallizes over time.

The thing about Abigail is that her trust model is simple: I trust her because I built her and I control the keys. That's fine for a personal agent. It's completely inadequate for anything else.

If my employer asked "who authorized this agent to access our systems?" the honest answer would be "me, from my home office, using keys I generated myself." That answer does not survive contact with an audit committee.

---

## Orion Dock: The Container Agent

**[ILLUSTRATION: Scale Spectrum — Personal to Enterprise]**

Orion Dock is the framework I wrote about in the last article — a Rust-based platform where agents earn their identity through a cryptographic birth ceremony. Ten crates. Docker deployment. Tiered skill sandboxing. The works.

Orion Dock solves a different problem than Abigail. It's built for scale — you can spin up multiple agents in containers, each with its own Ed25519 identity, its own constitutional document, its own skill permissions. The birth ceremony means every agent has verifiable provenance. The master key signs each agent's identity into a trust chain.

This is the piece that could work for a company deploying AI agents as "employees" — containerized, sandboxed, cryptographically accountable. An agent fleet where you can prove which agent did what, when, and with whose authority.

But there's still a gap. Orion Dock generates its own master key. It manages its own trust hierarchy. It's a self-contained kingdom. And enterprises don't want self-contained kingdoms. They want everything to flow through their identity provider, their SSO, their existing governance structure.

Amateurs shouldn't do crypto, and identity in the cloud is crypto. So where does the enterprise trust chain start?

---

## SAO: The Missing Layer

**[ILLUSTRATION: SAO Architecture]**

SAO — Secure Agent Orchestrator — is the piece I didn't know I was missing until I tried to connect the other two.

Here's what it actually does. SAO is an Axum server backed by PostgreSQL that manages three things: a full cryptographic vault, an agent registry, and the bridge to your enterprise identity.

**The Vault.** Every secret in the ecosystem lives here — Ed25519 signing keys, API provider tokens (OpenAI, Anthropic, Google, whatever), GPG keys, OAuth tokens. All encrypted at rest with AES-256-GCM. All audited on every access. Agents don't store their own credentials. They authenticate to SAO and receive what they're authorized to use. If an agent is compromised, you revoke it in one place.

**The Registry.** Every agent registers with SAO and gets verified against the master key's trust chain. SAO knows which agents exist, what state they're in, what capabilities they've been granted, and when they last checked in. Think of it as the HR system for your agent fleet, except it actually works.

**The Identity Bridge.** This is the part that matters. SAO supports WebAuthn/FIDO2 for local authentication — Windows Hello, security keys, biometrics. And it supports OIDC for enterprise SSO — Entra ID, Auth0, Google, whatever your company runs. Human users authenticate through your existing IDP. Agents authenticate through Ed25519 signatures. Both paths converge in the same authorization layer.

**[ILLUSTRATION: Identity Trust Chain]**

The trust chain looks like this: your enterprise IDP vouches for human administrators. Those administrators use SAO to provision and sign agent identities. SAO's master key signs each agent's Ed25519 key. When an agent authenticates to any service, the chain is traceable all the way back to your corporate directory.

That's the answer to "who issued the badge?" Your IDP did, through SAO, with a cryptographic proof chain your auditors can verify.

---

## How the Pieces Connect

Here's the ecosystem in practice:

**Personal use.** You run Abigail on your laptop. She connects to SAO to retrieve her API keys instead of storing them locally. Your personal SAO instance manages your vault. Simple.

**Team use.** Your team deploys Orion Dock with a few containerized agents. SAO manages the master key, provisions agent identities, and stores all the API credentials. Team members authenticate through WebAuthn. Every agent action is audited.

**Enterprise use.** SAO connects to your company's Entra ID through OIDC. Administrators are authenticated against your corporate directory. They use SAO's dashboard to provision agents, assign key sets to "Hives" (logical groups of agents that share permissions), configure which agents can access which provider APIs, and review audit logs. The agent fleet runs in Orion Dock containers. When an agent needs an Anthropic API key to process a claim, it authenticates to SAO with its Ed25519 signature, SAO verifies the trust chain, checks the agent's permissions, and returns the key. The whole interaction is logged.

No API keys in environment variables. No secrets in config files. No "well, someone on the team has the credentials somewhere." The vault is the single source of truth, and the IDP is the single source of authorization.

---

## The First-Run Experience

**[ILLUSTRATION: SAO Dashboard]**

I built a React frontend for SAO because nobody's going to manage an agent fleet from curl commands. The first time you spin up SAO, it walks you through a setup wizard: create an admin account with WebAuthn, set a vault passphrase, generate the master signing key. After that, the dashboard gives you the vault, agent registry, SSO configuration, user management, and audit logs.

It's not pretty yet. It's functional. There's a difference, and I'm aware of which side I'm on.

The SSO configuration page lets you add OIDC providers — plug in your issuer URL, client ID, client secret, and scopes. Once configured, anyone in your org can sign in through your corporate identity and interact with the agent fleet at whatever permission level they've been granted.

---

## What I'm Still Arguing With Myself About

I'll be honest about the parts that aren't settled.

**Agent-to-agent trust.** Right now, SAO manages agent-to-human and agent-to-service trust. Agent-to-agent communication is still routed through SAO as a hub. I'm not sure if that's the right long-term architecture or if agents need to establish peer trust directly. The hub model is simpler to audit. The mesh model is more resilient. I haven't decided.

**Key rotation at scale.** The vault supports key rotation, but rotating credentials across a fleet of 50 agents without downtime is an operations problem I haven't fully solved. I have ideas. They might be bad ideas.

**The ethical layer.** There's a fourth piece in the architecture — Ethical_AI_Reg — that's supposed to provide ethical evaluation for agent decisions. SAO has the bridge endpoints built. The ethical scoring model isn't where I want it yet. The TriangleEthic framework I've been developing balances three ethical traditions, but turning philosophy into scoring functions is exactly as hard as it sounds.

**Offline resilience.** What happens when SAO is unreachable and an agent needs credentials? Right now, the agent stops. That's the safe answer but not the practical one. Some form of credential caching with expiry is probably necessary. I don't love it.

---

## The Right Questions

I started this series with an aphorism an AI wrote: *A system is a promise you keep at scale.* The more I build, the more I think the promise isn't about what agents can do. It's about who authorized them to do it.

The identity problem in AI isn't a technical problem. OAuth is to identity as AC is to electricity — we solved the protocol decades ago. The problem is that we keep treating agent identity as an afterthought, something you bolt on after the demo works, and then we're surprised when the audit fails or the key leaks or nobody can tell which agent did what.

SAO isn't the answer to everything. It's the answer to one question: where does the trust chain start? It starts with your identity provider, flows through a cryptographic vault, and extends to every agent in your fleet through verifiable signatures.

Whether that's the *right* answer depends on questions I'm still asking. But I think they're the right questions.

The code is at github.com/jbcupps/SAO. Orion Dock is at github.com/jbcupps/Orion_Dock. Abigail is at github.com/jbcupps/abigail. All works in progress. All deeply flawed in ways I'm mostly aware of.

As always, just shouting into the void.

---

*I try to ask the right questions. Sometimes I even build things to test whether I asked the right ones. If you're thinking about agent identity, enterprise AI governance, or you just want to tell me my key rotation strategy is bad — I'd like to hear from you.*

#AI #AutonomousAgents #SecurityArchitecture #ZeroTrust #Rust #EnterpriseAI #IdentityManagement #InfoSec #AIGovernance
