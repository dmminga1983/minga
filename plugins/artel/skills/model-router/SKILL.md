---
name: model-router
description: Before delegating to a subagent (Task/Agent), picks which model (Haiku/Sonnet/Opus), effort level and verification depth to use — by the task's complexity and risk — then refines its own thresholds from a log of outcomes. Use before Agent/Task delegation, or say "маршрутизатор моделей", "model router".
---

# Model router / Маршрутизатор моделей

Reply in the user's language.

When work gets split up and handed to subagents (via Agent/Task), don't send everything through the same model on max settings "just in case" — that's expensive and doesn't make the result any more reliable. And don't cut costs by picking a model by hand with no check on the result — that's cheap, but unreliable: something will eventually come back wrong, and nothing will catch it.

Before every Agent/Task call, rate the piece of work along two axes and assign a model, effort level and verification method using the table below.

## 1. Rate complexity and risk

**Complexity** — how much actual reasoning is required: finding and reading a file isn't complexity; designing an architecture, tracking down a non-obvious bug, or reconciling conflicting requirements is.

**Risk** — what happens if the result turns out wrong: does it break production, cost money, affect a legally binding document, compromise security, or trigger an irreversible action — or does it just need to be regenerated.

## 2. Routing table (baseline — see section 5 on refining it)

| Complexity | Risk | Model | Effort | Verification |
|---|---|---|---|---|
| low | low | Haiku | low / default | not needed — result is self-evident (a typo, formatting, a simple lookup) |
| medium | medium | Sonnet | medium | quick pass — result matches the request, nothing obviously broken |
| high | high | Opus (or Sonnet at high effort if Opus isn't available) | high | full review — line-by-line against every requirement before handing it off further |

If torn between two rows, pick the more expensive one: underestimating risk usually costs more than a few extra cents in tokens.

## 3. Escalating on a failed check

If a cheap attempt (Haiku, or Sonnet at low effort) fails verification, don't ship it as-is: bump it up a tier (Sonnet → Opus) and retry. Cheap first, verify, didn't pass → go up a tier. Never burn an expensive call without verifying its result afterward either — that is how money gets spent with no guarantee of a correct result.

This escalation rule is what guarantees a correct final result regardless of how well-tuned the router currently is — the savings come from picking a cheaper starting point, never from weakening the check.

## 4. Important caveat

This never changes the model of the current session — the model answering right now is whatever was chosen when the session started. Routing only applies to work delegated further via Agent/Task; if nothing gets delegated, there is nothing to route.

## 5. Log and self-improvement

There are no weights here — the practical equivalent is a log of actual outcomes, reviewed periodically to refine the table's thresholds, with any change proposed to the user for confirmation rather than applied silently.

After every verification (pass or escalation), add one line to `.minga-router-log.md` at the root of the current project (skip this step if there is no suitable project — don't create the file for its own sake):
```
- 2026-10-09: [task type] haiku→failed→sonnet passed; reason: ...
- 2026-10-09: [task type] sonnet passed on the first try
```

Once enough entries have built up (rule of thumb: 15–20 new ones since the last review) and a clear pattern shows up — a task type that regularly fails the cheap attempt and always needs escalation, or one where the expensive tier keeps passing with room to spare — propose the user an updated version of this skill with the exception added to the table and a short justification (how many observations, what pattern). Never change the routing rules silently; the user confirms, like any other skill update.
