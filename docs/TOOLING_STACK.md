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
- Rust toolchain (cargo, rust-analyzer) is available via Claude CLI or Cursor.
- Docker/Kubernetes ready for Orion_dock testing.
- Tauri ready for Abigail desktop builds.

## Preferences & Constraints for the Architect
- Prefer suggestions that leverage Cursor IDE or Claude CLI for code changes.
- Avoid Gemini CLI for production-path code (known bugginess).
- All PRs and commits will be made directly by me (solo builder).
- Keep this file updated whenever any tool/subscription changes.

This file is the single source of truth. Update it first, then continue normal work.
