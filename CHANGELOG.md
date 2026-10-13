# Changelog

## 1.1.0 — 2026-10-13

Project renamed: Artel → Minga. All plugin names, commands (`/artel:...` → `/minga:...`), directories (`plugins/artel*` → `plugins/minga*`), the guard's env var (`ARTEL_GUARD` → `MINGA_GUARD`) and false-positive marker (`artel:allow` → `minga:allow`) were updated accordingly. No functional change. The repository itself was renamed on GitHub; old `artel` links redirect automatically.

## 1.0.0 — 2026-10-08

Minga becomes a universal marketplace.

- Core `minga`: 12 agents (planner, architect, verifier, code-reviewer, security-auditor, test-engineer, debugger, build-fixer, refactorer, performance-engineer, researcher, docs-writer) and 17 skills; always-on cost ≈ 3.0k tokens.
- Prompt-injection hardening: `/minga:injection-defense` skill (threat model, architecture checklist, red-team tests) and an "Untrusted content" rule in every agent and in the skills that read external content.
- 13 stack packs, one skill each with reference files: Python, TypeScript, Go, Rust, JVM, .NET, PHP, Ruby, Swift, Flutter, C/C++, data, DevOps.
- `minga-web` and `minga-content` packs (websites, bots, video scripts, unit economics).
- Guard hook moved into the core plugin (`plugins/minga/hooks`).
- Removed domain-specific skills; the project is no longer tied to one profession.
- Validator enforces context budgets and documentation coverage; stack pack authoring guide.

## 0.1.0 — 2026-10-08

First release (single plugin).
