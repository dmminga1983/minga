# Changelog

## 1.1.0 — 2026-10-13

- Core `artel`: new `/artel:model-router` skill — before delegating to a subagent (Task/Agent), picks model, effort and verification depth by complexity and risk, and logs outcomes to refine its own thresholds. Core is now 12 agents and 18 skills.
- `artel-content`: new `/artel-content:humanize-text` skill — final polish pass that strips "AI smell" from finished text (captions, tags, reels, lyrics, tables/reports, spoken scripts) before publishing.

## 1.0.0 — 2026-10-08

Artel becomes a universal marketplace.

- Core `artel`: 12 agents (planner, architect, verifier, code-reviewer, security-auditor, test-engineer, debugger, build-fixer, refactorer, performance-engineer, researcher, docs-writer) and 17 skills; always-on cost ≈ 3.0k tokens.
- Prompt-injection hardening: `/artel:injection-defense` skill (threat model, architecture checklist, red-team tests) and an "Untrusted content" rule in every agent and in the skills that read external content.
- 13 stack packs, one skill each with reference files: Python, TypeScript, Go, Rust, JVM, .NET, PHP, Ruby, Swift, Flutter, C/C++, data, DevOps.
- `artel-web` and `artel-content` packs (websites, bots, video scripts, unit economics).
- Guard hook moved into the core plugin (`plugins/artel/hooks`).
- Removed domain-specific skills; the project is no longer tied to one profession.
- Validator enforces context budgets and documentation coverage; stack pack authoring guide.

## 0.1.0 — 2026-10-08

First release (single plugin).
