---
name: graph
description: Code knowledge graph via the local minga-graph CLI. Use to map an unfamiliar codebase, find where something lives, trace how two parts connect, or check what breaks before changing or deleting code, instead of grepping many files. Триггеры - «граф кода», «что сломается если поменять», «как связано», «карта проекта».
---

# Code knowledge graph (minga-graph)

Reply in the user's language.

[minga-graph](https://github.com/dmminga1983/minga-graph) builds a deterministic graph of the project (Python, JS/TS, SQL, Markdown, dependency manifests) on the user's machine. Nothing leaves the machine: no network, no telemetry, no query logs. Every edge carries `EXTRACTED` or `INFERRED`, a reason and a `file:line`.

## 1. Make sure the CLI exists

Run `minga-graph --version`. If it is missing, tell the user to install it once and stop:

```
pipx install git+https://github.com/dmminga1983/minga-graph
```

(or `python -m pip install --user git+https://github.com/dmminga1983/minga-graph`). Requires Python 3.10+.

## 2. Build or refresh

`minga-graph build .` (incremental: only changed files are re-parsed). Add `--lang ru` when the user writes in Russian. Output goes to `minga-out/`: `GRAPH_REPORT.md`, `graph.json`, `graph.html` (offline, opens in any browser). Suggest adding `minga-out/` to `.gitignore` if it is not there.

For an overview, read `minga-out/GRAPH_REPORT.md` and summarise: possible secrets first (if any), then key nodes, subsystems, import cycles, surprising links, unused candidates.

## 3. Answer questions from the graph

Prefer these over reading many files; open only the `file:line` locations they print.

| Need | Command |
|---|---|
| Where is the code for a question (EN or RU) | `minga-graph query "how does login reach the database"` |
| Everything about one symbol, file or table | `minga-graph explain UserService` |
| How two things connect | `minga-graph path LoginForm users` |
| What breaks if X changes | `minga-graph impact parse_config --depth 3` |
| Structure changes since the last build | `minga-graph diff` |
| Import cycles | `minga-graph cycles` |
| Hard-coded secrets | `minga-graph secrets .` (exit 1 on findings) |

Exit code 3 means an ambiguous name: retry with one of the printed ids (for example `sym:src/a.py::Foo`). Exit code 4 means not found: try a shorter name or `query`.

## 4. Trust levels

- `EXTRACTED`: explicit in the source (definition, import binding, `self.x`/`this.x`, SQL `REFERENCES`, Markdown link).
- `INFERRED`: matched by a unique project-wide name. Open the location before relying on it for a risky change.
- Ambiguous names are never linked, so a missing edge is not proof of no relationship.

## 5. After editing

Rebuild (`minga-graph build .`) and run `minga-graph diff` to show what the change did to the structure. Before deleting code, run `impact` and list every dependent you found.
