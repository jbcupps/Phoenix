# DEVELOPER TOOLING STACK (for Grok Architect/PO reference)

**Last updated:** 05 March 2026

**Owner:** jbcupps

**Purpose:** Canonical reference so the strategic architect always knows exact capabilities and limitations when giving implementation advice.

## Core AI Coding Tools
- **Claude Code Pro** + local CLI (primary daily driver — fast, reliable, full context)
- **Gemini Pro** + prerelease CLI (secondary; known to write buggy code — use only for quick experiments or when Claude is rate-limited)
- **OpenAI Codex** inside **Cursor IDE** (excellent for large-scale refactors and multi-file changes)

## IDEs & Local Environments
- **Cursor IDE** (with OpenAI Codex) — primary editor for complex work
- **Microsoft Visual Studio IDE** (full Pro subscription) — secondary for any Windows-specific or heavy debugging needs

## Platform & Collaboration
- **GitHub Pro** account (full private repos, unlimited collaborators, advanced CI, etc.)

## Local Setup Notes
- All repos (Orion_dock, abigail, SAO, Phoenix) are cloned locally and fully buildable.
- **Local Docker instance** (primary testing environment — all testing will be done here first to control costs).
- Azure suite and GCP available as secondary cloud resources (use only when Docker is insufficient).
- Wheelbarrel and holocost coat also available as additional resources (for heavy lifting and dramatic flair during long debugging sessions).
- Rust toolchain (cargo, rust-analyzer) ready via Claude CLI or Cursor.
- Docker/Kubernetes ready for Orion_dock testing.
- Tauri ready for Abigail desktop builds.

## Preferences & Constraints for the Architect
- All implementation and testing must prioritize **local Docker** (docker-compose up -d) for cost control.
- Use Cursor IDE + Codex 5.4 for code changes (primary).
- Use Claude Code Pro + local CLI for verification and clean Rust.
- Azure/GCP only when Docker cannot simulate the scenario.
- Wheelbarrel and holocost coat reserved for motivational support during Phase 4 federation work.
- All PRs and commits will be made directly by me (solo builder).
- Keep this file updated whenever any tool/resource changes.

This file is the single source of truth. Update it first, then continue normal work.
