# Agent extension contract: routes and environment bindings

This document is for **AI agents, Lead Agents and workflow orchestrators** that maintain or adapt Browser Tool Compass. It is not a human click-by-click setup guide.

Browser Tool Compass separates **portable route semantics** from **environment-specific tool bindings**. The default rule is:

> **Map a new tool to an existing C1–C11 route first. Add a new route class only when the existing taxonomy cannot preserve the task's surface, authority, evidence, binding, visibility or data-boundary semantics.**

A new product, plugin, MCP tool, CLI, browser integration or provider is **not** automatically a new route.

## Agent classification contract

When an agent encounters a browser-related capability that is not represented in the current environment, it MUST classify the maintenance change before editing project files.

### `BINDING_EXTENSION`

Use when an existing C1–C11 route already represents the required task semantics.

The agent SHOULD keep the C-class unchanged and add or update the environment adapter/provider binding. It must identify the exact tool, trusted instructions, bounded read-only probes, gate semantics, profile/session binding requirements, visibility constraints, data boundary and failure behavior.

### `PROVIDER_REFINEMENT`

Use when the existing route semantics are correct but a provider needs extra runtime, consent, ownership, profile-binding or other gates.

The agent SHOULD keep the route ID and add only the provider-specific binding/gates required by that environment. Provider metadata in `references/route-contract.json` remains offline semantic metadata; it does not make the provider callable.

### `NEW_ROUTE_PROPOSAL`

Use only when all of the following are true:

- the required browser surface or execution boundary is materially different from every existing C-class;
- mapping it to an existing route would change that route's meaning or weaken a hard constraint;
- the required evidence/authorization gates cannot be expressed as a provider refinement of an existing route; and
- at least one concrete task case demonstrates why a new semantic category is necessary.

If the difference is only product/vendor/version/install-path specific, the agent MUST NOT create a new route. Do not create C12 merely because an environment exposes a twelfth browser product.

## Required agent decision record

Before mutating taxonomy or bindings, produce a concise maintenance record:

```text
CAPABILITY_OR_TOOL:
TARGET_ENVIRONMENT:
CLASSIFICATION: BINDING_EXTENSION | PROVIDER_REFINEMENT | NEW_ROUTE_PROPOSAL
EXISTING_ROUTE_CANDIDATES:
WHY_EXISTING_ROUTE_IS_SUFFICIENT_OR_INSUFFICIENT:
HARD_CONSTRAINTS_TO_PRESERVE:
REQUIRED_EVIDENCE_GATES:
REQUIRED_AUTHORIZATION_GATES:
FILES_EXPECTED_TO_CHANGE:
```

This is a maintenance decision artifact, not runtime evidence.

## Existing-route binding contract

For `BINDING_EXTENSION` or `PROVIDER_REFINEMENT`, the agent MUST preserve portable route semantics unless evidence shows they are insufficient.

The resulting binding should define:

- adapter identity/version and supported environment;
- supported route IDs and explicit unsupported routes;
- exact currently exposed tool plus trusted tool instructions;
- discovery rules when paths/tool names can vary;
- bounded read-only probes and the proposition each probe proves;
- expected response fields and `PASS` / `FAIL` / `BLOCKED` / `UNKNOWN` / `NOT_APPLICABLE` interpretation;
- identity/profile/session evidence where required;
- local/remote data-boundary rules and minimal returned metadata;
- stop, cleanup and ownership behavior.

The agent MUST NOT install, start, repair, reconfigure or weaken trust controls merely to manufacture a passing probe.

## New-route change contract

For `NEW_ROUTE_PROPOSAL`, the agent MUST update the portable contract consistently rather than editing one file in isolation:

1. `SKILL.md` — route ID, portable name and selection semantics.
2. `SKILL.zh-tw.md` — semantically aligned Traditional Chinese companion.
3. `references/routing-matrix.md` and `.zh-tw.md` — required gates, authorization gates, pass evidence, failure meaning and stop conditions.
4. `references/route-contract.json` — machine-readable route metadata. Increment `schema_version` only when the JSON contract shape or interpretation changes, not merely because a route row is added.
5. `examples/route-cases.md` and `.zh-tw.md` — at least one positive selection case and one ambiguity/failure/non-selection case.
6. `README.md` and `README.zh-tw.md` — Agent route map and all C-range references.
7. Re-run consistency checks across route IDs, names and gate semantics and verify that no wording implies executable browser automation.

Route IDs are stable compatibility identifiers. The agent MUST NOT renumber existing classes merely to insert a new one. Append the next ID unless a separately governed breaking migration is explicitly authorized.

## Acceptance conditions

The maintaining agent may mark the extension ready only when:

- the binding-vs-route classification is explicit and justified;
- environment-specific facts remain outside portable route semantics unless they truly define a new semantic boundary;
- probes are bounded and do not mutate state to create a pass;
- authority, capability, binding, execution and application outcome remain separate propositions;
- explicit surface/profile/visibility/data-boundary constraints survive fallback;
- synthetic examples cover the intended selection and at least one boundary case;
- English and Traditional Chinese maintained documents remain semantically aligned;
- the repository still describes a decision/router skill, not a browser automation runtime.

[Traditional Chinese companion](extending-routes.zh-tw.md)
