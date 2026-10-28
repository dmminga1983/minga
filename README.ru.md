# Minga

**Лёгкий и безопасный маркетплейс агентов и навыков для Claude Code.** Ставите небольшое ядро (12 агентов, 17 навыков, сторож утечек), а потом добавляете только нужные стек-паки: Python, TypeScript, Go, Rust, Java/Kotlin, .NET, PHP, Ruby, Swift, Flutter, C/C++, данные, DevOps, веб, контент.

[English version →](README.md)

```
/plugin marketplace add dmminga1983/minga
/plugin install minga@minga
/plugin install minga-python@minga      # добавьте нужные паки
```

Затем введите `/minga:start` — Minga спросит, что вы хотите сделать, и подскажет один следующий шаг.

---

## Почему Minga

Большие наборы вроде [ECC](https://github.com/affaan-m/ECC) ставят сотни навыков сразу. Claude Code ограничивает список навыков примерно 1% окна контекста: при переполнении описания отбрасываются, навыки перестают срабатывать, а постоянная «цена» платится в каждой сессии. Minga устроен иначе:

| | ECC (замер, окт. 2026) | Minga |
|---|---|---|
| Постоянный расход контекста | ~44,8 тыс. токенов | **~3,0 тыс. токенов** на ядро; ~180 на стек-пак |
| Структура | один большой плагин, ~68 агентов, ~290 навыков | ядро + 15 необязательных паков; **один навык на стек**, справочники читаются по требованию |
| Безопасность | сканер конфигурации | **сторож, включённый с установки**: блокирует коммиты и пуши с ключами, токенами, `.env` и файлами ключей |
| Независимая проверка | — | агент `verifier`, который не оценивает собственную работу |
| Языки | английский | агенты отвечают на языке пользователя; навыки срабатывают на русском и английском |
| Сеть и деньги | платные тарифы, реферальные ссылки | без сетевых вызовов, платных действий, рекламы и телеметрии |
| Зависимости | — | нет; Node ≥ 18 нужен только сторожу, при сбое он не мешает |

Цифры токенов получены командой `claude plugin details`; их можно воспроизвести после установки.

## Цикл Minga

1. **Спланировать** многошаговую работу (`/minga:plan-first`).
2. **Сделать** маленькими проверяемыми шагами.
3. **Проверить**: запустить, открыть, сверить с источниками (`/minga:verify-done`).
4. **Безопасно опубликовать**: сначала скан секретов, потом push (`/minga:secure-publish`).

## Ядро (`minga@minga`)

### Агенты

Агент — помощник с одной ролью и собственным контекстом. Claude подключает его сам, либо попросите: «пусть `security-auditor` проверит репозиторий». Если установлен стек-пак, агенты подгружают его для найденного стека.

| Агент | Что делает |
|---|---|
| `planner` | Превращает размытую задачу в план с критерием готовности |
| `architect` | Выбирает стек и структуру, фиксирует компромиссы |
| `verifier` | Независимо проверяет готовое: запускает, открывает, сверяет факты |
| `code-reviewer` | Ревью по важности: баги, безопасность, тесты, ясность |
| `security-auditor` | Секреты в истории, `.gitignore`, GitHub Actions, зависимости |
| `test-engineer` | Пишет тесты, сначала тест |
| `debugger` | Ищет первопричину, а не симптом |
| `build-fixer` | Чинит сборку и упавший CI минимальной правкой |
| `refactorer` | Переделывает код малыми шагами под защитой тестов |
| `performance-engineer` | Сначала измеряет, потом оптимизирует реальное узкое место |
| `researcher` | Исследование с источниками: каждое утверждение со ссылкой |
| `docs-writer` | Понятные README и инструкции |

### Навыки

| Команда | Что делает |
|---|---|
| `/minga:start` | Подбирает агента, навык или пак под задачу |
| `/minga:plan-first` | План и критерий готовности до начала работы |
| `/minga:verify-done` | Проверка перед тем, как сказать «готово» |
| `/minga:code-review` | Ревью текущих изменений |
| `/minga:tdd` | Разработка через тесты |
| `/minga:debug` | Систематическая отладка |
| `/minga:refactor` | Рефакторинг без изменения поведения |
| `/minga:perf` | Профилирование, правка узкого места, повторный замер |
| `/minga:onboard` | Быстро разобраться в незнакомом проекте |
| `/minga:git-workflow` | Ветки, коммиты, pull request'ы |
| `/minga:ship` | Чек-лист релиза, changelog, тег |
| `/minga:research` | Исследование с источниками |
| `/minga:secure-publish` | Проверка секретов, `.gitignore` и Actions перед публикацией |
| `/minga:secrets-setup` | Настройка `.env`, `.env.example` и защиты ключей |
| `/minga:repo-audit` | Аудит чужого репозитория или плагина перед установкой |
| `/minga:injection-defense` | Защита LLM-приложений, ботов и агентов от prompt injection, чек-лист и тесты |
| `/minga:make-skill` | Создать свой навык или агента |

## Стек-паки

Каждый пак — один навык: как определить проект, настройки по умолчанию, идиомы, ошибки, тесты, подводные камни безопасности, производительность, таблица ошибок сборки и чек-лист ревью; справочные файлы читаются по требованию.

| Установка | Команда | Охват |
|---|---|---|
| `minga-python@minga` | `/minga-python:python` | uv/poetry/pip, типизация, pytest, Django, FastAPI, Flask |
| `minga-typescript@minga` | `/minga-typescript:typescript` | Node, React, Next.js, Vue, Angular, Svelte, Express/Nest |
| `minga-go@minga` | `/minga-go:go` | модули, конкурентность, тесты, net/http, gRPC |
| `minga-rust@minga` | `/minga-rust:rust` | cargo, владение, async, ревью unsafe, axum |
| `minga-jvm@minga` | `/minga-jvm:jvm` | Java, Kotlin, Gradle/Maven, Spring Boot, Android |
| `minga-dotnet@minga` | `/minga-dotnet:dotnet` | C#, ASP.NET Core, EF Core |
| `minga-php@minga` | `/minga-php:php` | Composer, Laravel, Symfony |
| `minga-ruby@minga` | `/minga-ruby:ruby` | Bundler, Rails, RSpec |
| `minga-swift@minga` | `/minga-swift:swift` | SwiftPM/Xcode, SwiftUI, конкурентность |
| `minga-flutter@minga` | `/minga-flutter:flutter` | Dart, управление состоянием, виджет-тесты |
| `minga-cpp@minga` | `/minga-cpp:cpp` | CMake, санитайзеры, ошибки линковки |
| `minga-data@minga` | `/minga-data:data` | SQL, схемы, безопасные миграции, Redis |
| `minga-devops@minga` | `/minga-devops:devops` | Docker, Kubernetes, Terraform, защита Actions |
| `minga-web@minga` | `/minga-web:landing-page`, `/minga-web:seo-audit`, `/minga-web:telegram-bot` | агенты `web-builder`, `bot-builder` |
| `minga-content@minga` | `/minga-content:reel-script`, `/minga-content:youtube-script`, `/minga-content:unit-economics` | агенты `content-producer`, `business-analyst` |

## Наборы в отдельных репозиториях

Крупные наборы живут в своих репозиториях, со своими тестами и релизами. Маркетплейс Minga ссылается на них, поэтому ставятся они так же, как остальные.

| Установка | Что делает | Репозиторий |
|---|---|---|
| `minga-arena@minga` | Турнир субагентов, когда ответ недостаточно хорош: 16 агентов, атака, защита, судья, остаётся один ответ | [minga-arena](https://github.com/dmminga1983/minga-arena) |
| `minga-reels@minga` | Instagram Reels и YouTube Shorts на русском и английском: персонажи, оценка хуков, сценарии с таймингом, проверка подписей, разбор статистики | [minga-reels](https://github.com/dmminga1983/minga-reels) |
| `minga-youtube@minga` | Длинные видео на YouTube: сценарии, название и превью, монтажный лист, главы, удержание, SEO | [minga-youtube](https://github.com/dmminga1983/minga-youtube) |
| `minga-replica@minga` | Воссоздать приложение по принципу чистой комнаты и запустить как своё, для России и мира, App Store, Google Play и RuStore | [minga-replica](https://github.com/dmminga1983/minga-replica) |

```
/plugin install minga-reels@minga
```

## Сторож утечек

Ядро ставит хук, который срабатывает перед каждым `git commit` и `git push`:

- **блокирует**, если в изменении есть API-ключ, токен, приватный ключ или файл `.env`/ключа;
- **спрашивает** при значениях, похожих на пароль (в коде, конфиге, URL, JWT), при `--force` в `main`/`master` и при изменениях свыше 20 МБ, которые не удалось проверить полностью;
- понимает `cd dir && git commit`, кавычки, `bash -c`, git-алиасы, push другой ветки или тега и `gh repo create --push`;
- ничего не меняет в репозитории и никуда ничего не отправляет;
- при сбое не мешает работе.

Находит ключи AWS, GitHub, Anthropic, OpenAI, Google, Slack, Stripe и другие, токены ботов, приватные ключи и файлы `.env`, `.pem`, `id_rsa`.

- Ложное срабатывание: добавьте в строку комментарий `minga:allow`.
- Отключить: запустите Claude Code с переменной окружения `MINGA_GUARD=off` (или задайте её в `env` настроек Claude Code). Указать её внутри команды сознательно не работает.
- Проверить весь репозиторий с историей: `/minga:secure-publish` или `node plugins/minga/hooks/scripts/scan-secrets.mjs --history <репозиторий>`.
- Это страховка, а не гарантия — см. [SECURITY.md](SECURITY.md). Включите также secret scanning и push protection на GitHub.

Нужен **Node.js 18+**. Без Node сторож молча неактивен, остальное работает.

## Удаление

```
/plugin uninstall minga@minga
/plugin marketplace remove minga
```

## В приложении Claude (claude.ai)

Навыки совместимы с приложением Claude. Соберите ZIP: `python3 tools/package_skills.py` (все) или `python3 tools/package_skills.py python`, затем загрузите в настройках Claude. Агенты и сторож работают только в Claude Code.

## Участие в проекте

```bash
npm run validate   # структура, frontmatter, бюджеты, покрытие документацией
npm test           # тесты сторожа и сканера
npm run scan       # секреты в файлах и истории
claude plugin validate . --strict
```

Новый стек-пак: [docs/STACK_PACK_GUIDE.md](docs/STACK_PACK_GUIDE.md) и [CONTRIBUTING.md](CONTRIBUTING.md).

## Важно

Агенты помогают, но не заменяют специалиста. Юридические, налоговые и критичные для безопасности выводы проверяйте сами, а персональные данные обезличивайте перед отправкой в любой ИИ-сервис. О найденной уязвимости сообщайте по [SECURITY.md](SECURITY.md).

## Лицензия

[MIT](LICENSE)
