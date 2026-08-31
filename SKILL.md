---
name: browser-tool-router
description: Choose and diagnose browser tool routes by task, required surface, profile, and evidence gates. Use when browser route selection is unclear; this skill does not install, repair, or operate browser tools itself.
---

# Browser Tool Router

Choose the narrowest authorized route that can produce the requested evidence. This portable decision layer defines semantic gates; a separately approved environment adapter supplies actual tool bindings and read-only probes. Without an approved adapter and successful current probes, runtime availability remains `UNKNOWN` unless direct evidence establishes `FAIL` or `BLOCKED`.

## Decide before dispatch

1. Capture the task and its hard constraints: explicit tool/surface, intended tab, profile/session, required visibility, local or remote data boundary, and allowed operations. Do not weaken them to obtain a passing route.
2. Apply the active instruction hierarchy and authorization first. Capability discovery and passing evidence never grant authority.
3. Select the route below, then read only its row in [the routing matrix](references/routing-matrix.md). Load [the adapter and evidence contract](references/adapter-evidence-contract.md) when binding tools or evaluating uncertain evidence.
4. Run only an authorized adapter's read-only probes, inside the stated scope. Record each gate independently in the current task/context. Discovery alone is not a runtime pass.
5. Dispatch to the selected tool's own instructions only after required gates pass. The downstream action needs its own authorization; route availability is not action acceptance.

| Class | Portable route | Select for |
|---|---|---|
| C1 | Public retrieval | Public unauthenticated content; no rendered UI required |
| C2 | Browser developer inspection | Console, network, performance, DOM or accessibility evidence |
| C3 | Session automation | One-off tabs, interaction or screenshots without project-owned tests |
| C4 | Project-owned browser tests | Repeatable regression, CI, traces; verified project ownership |
| C5 | Host-integrated visible browser | Explicit host application's browser surface or user-facing tab |
| C6 | User-profile integration | Existing user tabs or extension context through a profile integration |
| C7 | Existing authenticated browser connection | Authenticated acceptance through a connection to the authorized existing profile |
| C8 | Remote browser service | Explicitly scoped remote execution or approved remote profile |
| C9 | Local browser CLI smoke | Visual smoke against an already available development target |
| C10 | Desktop interaction | Desktop UI or an authorized last resort when browser APIs cannot fulfill the task |
| C11 | Extension project scaffolding | New extension project or extension governance; runtime verification uses other routes |

Authenticated acceptance generally selects C7; an explicit integration requirement may select C6. Neither class authorizes consent or writes. C6 and C7 must prove current binding to the intended profile/session, not merely a working transport. A loopback or file target alone does not require C5; use the task's evidence and surface requirements.

## Gates, fallback and stopping

- C5 requires separate `installed`, `runnable`, and `visible` passes. Visible means the intended controlled tab is exposed on the requested user-facing surface. Hidden control is insufficient.
- A failed gate stops that route. Consider alternatives only if they preserve every hard constraint and the task's authority permits them. State the changed route and evidence limitations before using it. An explicitly required surface/profile must not be silently substituted; if no alternative satisfies it, report `BLOCKED`.
- C8 is never an automatic substitute for a local tool, local file, local service or user profile. Remote processing requires explicit scope and a data-boundary check.
- Do not install tooling, launch/restart services, enable extensions, mutate configuration, change trust controls, edit caches, or copy profiles to make a gate pass. Report the boundary and a separately governed next action. Never bypass login, consent, paywalls, security warnings, CAPTCHA, password-manager flows, or user checkpoints.
- Do not read or record cookies, tokens, credentials, session storage or sensitive page contents as probe evidence. Use minimal sanitized metadata and opaque identifiers.
- Refresh affected evidence after tool/runtime, connection, profile, target or visibility changes. Historical reports and static files may guide discovery but cannot prove current runtime success.
- Keep transport, profile binding, visibility and application behavior separate. An application interaction failure does not erase an already-proven transport gate or prove the application passed.

## Decision record

Keep a concise record when dispatching or blocked:

```text
Task and hard constraints:
Authority and allowed operations:
Primary route and adapter identity:
Gate results, sanitized evidence and context:
Fallback eligibility (or why none):
Sensitive checkpoints and expected outcome evidence:
```

Use gate statuses `PASS`, `FAIL`, `BLOCKED`, `UNKNOWN`, `NOT_APPLICABLE`. Use factual classifications `VERIFIED`, `INFERRED`, `UNKNOWN` separately; an inferred binding is not a passed binding. See [synthetic route cases](examples/route-cases.md) for ambiguous and adversarial decisions. [The compact route contract](references/route-contract.json) supports offline consistency checks; it is not a browser implementation or runtime evidence.

English is authoritative. Traditional Chinese companions: [skill](SKILL.zh-tw.md), [matrix](references/routing-matrix.zh-tw.md), [adapter contract](references/adapter-evidence-contract.zh-tw.md), [cases](examples/route-cases.zh-tw.md).
