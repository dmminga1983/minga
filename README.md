# Minga

**The Claude Code team that plans, builds, verifies and ships your code — and never lets a secret slip out.**

Most agent marketplaces throw hundreds of skills at you and hope a few stick. Minga does the opposite: a tight 12-agent core (~3k tokens, always on) plus 16 optional packs you add only when you need them — one skill per stack, not one per framework. A guard hook blocks leaking API keys, tokens and `.env` files before they ever reach a commit, and a dedicated `verifier` agent double-checks finished work instead of grading its own homework. Free, MIT-licensed, zero network calls, zero telemetry.

[Русская версия →](README.ru.md)

```
/plugin marketplace add dmminga1983/minga
/plugin install minga@minga
/plugin install minga-python@minga      # add the packs you need
```

Then type `/minga:start` — Minga asks what you want to do and points to one next step.

---

## Why Minga

Big toolkits such as [ECC](https://github.com/affaan-m/ECC) ship hundreds of skills at once. Claude Code caps the skill listing at about 1% of the context window, so on overflow most descriptions are dropped and skills stop triggering — and the always-on cost is paid in every session. Minga makes different choices:

| | ECC (as measured, Oct 2026) | Minga |
|---|---|---|
| Always-on context cost | ~44.8k tokens | **~3.0k tokens** for the core; ~180 per stack pack |
| Structure | one big plugin, ~68 agents, ~290 skills | core + 16 optional packs; **one skill per stack** with reference files read on demand |
| Security | configuration scanner | **a guard that is on from install**: blocks commits and pushes containing keys, tokens or `.env`/key files |
| Independent checking | — | a `verifier` agent that never grades its own work |
| Languages | English | agents reply in the user's language; skills trigger in Russian and English |
| Network and money | paid tiers, referral links | no network calls, no paid actions, no ads, no telemetry |
| Dependencies | — | none; Node ≥ 18 only for the guard, which fails open |

Token numbers come from `claude plugin details`; reproduce them after installing.

## The Minga loop

1. **Plan** multi-step work first (`/minga:plan-first`).
2. **Build** in small, checkable steps.
3. **Verify**: run it, open it, check sources (`/minga:verify-done`).
4. **Publish safely**: secret scan first, push second (`/minga:secure-publish`).

## Core (`minga@minga`)

### Agents

An agent is a helper with one role and its own context. Claude delegates automatically, or ask: "have `security-auditor` check this repo". When a stack pack is installed, the agents load it for the detected stack.

| Agent | What it does |
|---|---|
| `planner` | Turns a vague task into a step-by-step plan with a definition of done |
| `architect` | Chooses stack and structure; records trade-offs |
| `verifier` | Independently checks finished work: runs it, opens it, checks facts |
| `code-reviewer` | Review ranked by severity: bugs, security, tests, clarity |
| `security-auditor` | Secrets in history, `.gitignore`, GitHub Actions, dependencies |
| `test-engineer` | Writes tests, test-first |
| `debugger` | Finds the root cause, not the symptom |
| `build-fixer` | Fixes broken builds and failing CI with the smallest change |
| `refactorer` | Restructures code in small steps with tests as a safety net |
| `performance-engineer` | Measures first, then optimises the real bottleneck |
| `researcher` | Research with sources: every claim linked |
| `docs-writer` | Clear READMEs and instructions |

### Skills

| Command | What it does |
|---|---|
| `/minga:start` | Picks the agent, skill or pack for your task |
| `/minga:plan-first` | Plan and definition of done before work starts |
| `/minga:verify-done` | Verification before reporting "done" |
| `/minga:code-review` | Review of the current changes |
| `/minga:tdd` | Test-driven development |
| `/minga:debug` | Systematic debugging |
| `/minga:refactor` | Behaviour-preserving refactoring |
| `/minga:perf` | Profile, fix the bottleneck, re-measure |
| `/minga:onboard` | Understand an unfamiliar codebase quickly |
| `/minga:git-workflow` | Branches, commits, pull requests |
| `/minga:ship` | Pre-release checklist, changelog, tag |
| `/minga:research` | Research with sources |
| `/minga:secure-publish` | Secrets, `.gitignore` and Actions check before publishing |
| `/minga:secrets-setup` | `.env`, `.env.example` and key protection |
| `/minga:repo-audit` | Audit someone else's repo or plugin before installing it |
| `/minga:injection-defense` | Protect LLM apps, bots and agents from prompt injection; red-team checklist |
| `/minga:make-skill` | Create your own skill or agent |

## Stack packs

Each pack is one skill: how to detect the project, defaults, idioms, errors, testing, security pitfalls, performance, a build-error table and a review checklist, plus reference files loaded on demand.

| Install | Command | Covers |
|---|---|---|
| `minga-python@minga` | `/minga-python:python` | uv/poetry/pip, typing, pytest, Django, FastAPI, Flask |
| `minga-typescript@minga` | `/minga-typescript:typescript` | Node, React, Next.js, Vue, Angular, Svelte, Express/Nest |
| `minga-go@minga` | `/minga-go:go` | modules, concurrency, testing, net/http, gRPC |
| `minga-rust@minga` | `/minga-rust:rust` | cargo, ownership, async, unsafe review, axum |
| `minga-jvm@minga` | `/minga-jvm:jvm` | Java, Kotlin, Gradle/Maven, Spring Boot, Android |
| `minga-dotnet@minga` | `/minga-dotnet:dotnet` | C#, ASP.NET Core, EF Core |
| `minga-php@minga` | `/minga-php:php` | Composer, Laravel, Symfony |
| `minga-ruby@minga` | `/minga-ruby:ruby` | Bundler, Rails, RSpec |
| `minga-swift@minga` | `/minga-swift:swift` | SwiftPM/Xcode, SwiftUI, concurrency |
| `minga-flutter@minga` | `/minga-flutter:flutter` | Dart, state management, widget tests |
| `minga-cpp@minga` | `/minga-cpp:cpp` | CMake, sanitizers, linker errors |
| `minga-data@minga` | `/minga-data:data` | SQL, schema design, safe migrations, Redis |
| `minga-devops@minga` | `/minga-devops:devops` | Docker, Kubernetes, Terraform, Actions hardening |
| `minga-web@minga` | `/minga-web:landing-page`, `/minga-web:seo-audit`, `/minga-web:telegram-bot` | agents `web-builder`, `bot-builder` |
| `minga-content@minga` | `/minga-content:reel-script`, `/minga-content:youtube-script`, `/minga-content:unit-economics` | agents `content-producer`, `business-analyst` |
| `minga-graph@minga` | `/minga-graph:graph` | code knowledge graph via [minga-graph](https://github.com/dmminga1983/minga-graph): query, explain, path, impact, diff, secrets |

## The leak guard

The core installs a hook that runs before every `git commit` and `git push`:

- **blocks** when the change contains an API key, token, private key or `.env`/key file;
- **asks** on password-like values (in code, config, URLs, JWTs), on `--force` pushes to `main`/`master`, and on changes over 20 MB it could not fully scan;
- understands `cd dir && git commit`, quoting, `bash -c`, git aliases, pushing another branch or tag, and `gh repo create --push`;
- never modifies the repository and never sends anything anywhere;
- fails open: if something goes wrong, it gets out of the way.

It detects AWS, GitHub, Anthropic, OpenAI, Google, Slack, Stripe and other keys, bot tokens, private keys, and files such as `.env`, `.pem` and `id_rsa`.

- False positive: add the comment `minga:allow` to that line.
- Disable: start Claude Code with the environment variable `MINGA_GUARD=off` (or set it in `env` in Claude Code settings). Setting it inside a command deliberately does not work.
- Scan a whole repo including history: `/minga:secure-publish`, or `node plugins/minga/hooks/scripts/scan-secrets.mjs --history <repo>`.
- It is a safety net, not a guarantee — see [SECURITY.md](SECURITY.md). Also enable GitHub secret scanning with push protection.

Requires **Node.js 18+**. Without Node the guard is silently inactive; everything else works.

## Uninstall

```
/plugin uninstall minga@minga
/plugin marketplace remove minga
```

## In the Claude app (claude.ai)

Skills are compatible with the Claude app. Build ZIP archives with `python3 tools/package_skills.py` (all skills) or `python3 tools/package_skills.py python`, then upload in Claude's settings. Agents and the guard work only in Claude Code.

## Contributing

```bash
npm run validate   # structure, frontmatter, budgets, documentation coverage
npm test           # guard and scanner tests
npm run scan       # secrets in files and history
claude plugin validate . --strict
```

New stack pack? See [docs/STACK_PACK_GUIDE.md](docs/STACK_PACK_GUIDE.md) and [CONTRIBUTING.md](CONTRIBUTING.md).

## Note

The agents assist; they do not replace a professional. Check legal, tax and security-critical conclusions yourself, and anonymise personal data before sending it to any AI service. To report a vulnerability, see [SECURITY.md](SECURITY.md).

## License

[MIT](LICENSE)
