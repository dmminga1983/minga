---
name: start
description: Shows what Minga can do and routes the user to the right agent, skill or stack pack for their task. Use when the user asks what Minga is, how to start, which command to use, or says "help" / "что ты умеешь" / "с чего начать" / "помоги выбрать".
---

# Minga — start here / с чего начать

Reply in the user's language. Ask what they want to get done, then point to **one** next step — not the whole catalogue.

## Core: task → what to use

| Task / Задача | Skill | Agent |
|---|---|---|
| Plan a task before doing it / Спланировать | `/minga:plan-first` | `planner` |
| Choose architecture / Выбрать архитектуру | — | `architect` |
| Check finished work / Проверить готовое | `/minga:verify-done` | `verifier` |
| Publish code safely / Безопасно выложить | `/minga:secure-publish` | `security-auditor` |
| Set up secrets / Настроить .env и ключи | `/minga:secrets-setup` | — |
| Vet someone else's repo / Проверить чужой репозиторий | `/minga:repo-audit` | `security-auditor` |
| Protect an LLM app or bot from prompt injection / Защита от промт-инъекций | `/minga:injection-defense` | `security-auditor` |
| Review code / Ревью кода | `/minga:code-review` | `code-reviewer` |
| Tests first / Тесты | `/minga:tdd` | `test-engineer` |
| Find a bug / Найти причину ошибки | `/minga:debug` | `debugger` |
| Fix a broken build / Починить сборку | — | `build-fixer` |
| Restructure code safely / Рефакторинг | `/minga:refactor` | `refactorer` |
| Slow code / Оптимизация | `/minga:perf` | `performance-engineer` |
| Understand a new codebase / Разобраться в проекте | `/minga:onboard` | `researcher` |
| Branches, commits, PRs / Git-процесс | `/minga:git-workflow` | — |
| Release a version / Выпустить релиз | `/minga:ship` | `verifier` |
| Research with sources / Исследование | `/minga:research` | `researcher` |
| README and docs / Документация | — | `docs-writer` |
| Make your own skill / Свой skill | `/minga:make-skill` | — |

## Stack packs (install only what you use)

Core agents load the matching pack automatically when it is installed. Install with `/plugin install <pack>@minga`.

| Pack | Use |
|---|---|
| `minga-python` | `/minga-python:python` — Django, FastAPI, Flask, data/ML |
| `minga-typescript` | `/minga-typescript:typescript` — Node, React, Next.js, Vue, Angular, Svelte |
| `minga-go` | `/minga-go:go` |
| `minga-rust` | `/minga-rust:rust` |
| `minga-jvm` | `/minga-jvm:jvm` — Java, Kotlin, Spring, Android |
| `minga-dotnet` | `/minga-dotnet:dotnet` — C#, ASP.NET Core, EF Core |
| `minga-php` | `/minga-php:php` — Laravel, Symfony |
| `minga-ruby` | `/minga-ruby:ruby` — Rails |
| `minga-swift` | `/minga-swift:swift` — iOS, SwiftUI |
| `minga-flutter` | `/minga-flutter:flutter` — Dart |
| `minga-cpp` | `/minga-cpp:cpp` — C, C++, CMake |
| `minga-data` | `/minga-data:data` — SQL, migrations, Redis |
| `minga-devops` | `/minga-devops:devops` — Docker, Kubernetes, Terraform, CI |
| `minga-web` | `/minga-web:landing-page`, `/minga-web:seo-audit`, `/minga-web:telegram-bot` (agents `web-builder`, `bot-builder`) |
| `minga-content` | `/minga-content:reel-script`, `/minga-content:youtube-script`, `/minga-content:unit-economics` (agents `content-producer`, `business-analyst`) |
| `minga-arena` | `/minga-arena:minga-arena`: a tournament of sub-agents when an answer is not good enough (own repository) |
| `minga-reels` | `/minga-reels:reels-hook`, `/minga-reels:reels-script` and 9 more: Instagram Reels and YouTube Shorts (own repository) |
| `minga-youtube` | `/minga-youtube:yt-script`, `/minga-youtube:yt-package` and 9 more: long-form YouTube (own repository) |
| `minga-replica` | `/minga-replica:replica-recon` and 10 more: rebuild an app and launch it as your own, Russia and global (own repository) |

If the needed pack is not installed, say so and give the install line.

## Always-on protection

Minga's guard hook blocks `git commit` / `git push` when the change contains something that looks like a secret (API keys, tokens, private keys, `.env` files) and asks before a force-push to `main`/`master`. To turn it off, start Claude Code with the environment variable `MINGA_GUARD=off`.

## The Minga way of working

1. **Plan** before multi-step work.
2. **Build** in small, checkable steps.
3. **Verify** with fresh eyes — run it, open it, check sources.
4. **Publish safely** — secrets scan first, push second.
