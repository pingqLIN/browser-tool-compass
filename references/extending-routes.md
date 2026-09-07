# Extending routes and environment bindings

Browser Tool Compass separates **portable route semantics** from **environment-specific tool bindings**. Most environments should extend the adapter layer, not the C-class taxonomy.

## First decision: binding or new route?

Use an existing C1–C11 route whenever the new browser tool can satisfy that route's meaning without weakening task constraints, authority, evidence, profile/session binding, visibility, or data-boundary requirements.

**A new tool is not automatically a new route.**

| Situation | Preferred change |
|---|---|
| A new product/tool implements the same task semantics as an existing C-class | Add or update an environment adapter / provider binding |
| A host exposes the same route through a different command, plugin, MCP tool or installation path | Update the environment adapter |
| A provider needs extra runtime, consent, ownership or profile-binding checks | Add provider-specific gates/binding while keeping the existing route when its core semantics still fit |
| No existing C-class can represent the required browser surface or execution boundary without changing its meaning | Propose a new route class |

Do not create C12 merely because an environment has a twelfth browser product.

## Adding a tool to an existing route

1. **Choose the semantic route.** Match the task surface and execution boundary to C1–C11 in `SKILL.md` and `references/routing-matrix.md`.
2. **Define the environment binding.** Record adapter identity/version, supported environment, exact exposed tool, route IDs, unsupported routes and trusted instructions.
3. **Define read-only probes.** State the exact probe, its side effects, the proposition it tests, and the response fields that constitute `PASS`, `FAIL`, `BLOCKED` or `UNKNOWN`. A probe must not install, start, repair or reconfigure tooling just to pass.
4. **Preserve separate authority and evidence.** Tool availability never grants action authority. Profile/session binding, visibility, remote data handling and provider consent remain independent gates where applicable.
5. **Test ambiguity and failure.** Verify that missing evidence stays `UNKNOWN`, a failed required gate stops the route, and fallback never silently changes explicit surface/profile/data-boundary constraints.

Provider-specific metadata may be represented in `references/route-contract.json` under `provider_bindings` when useful for offline consistency. That entry is semantic metadata only; it does not make the provider callable.

## When a new route class is justified

A new route class is justified only when all of the following are true:

- the required task surface or execution boundary is materially different from every existing C-class;
- mapping it to an existing route would change that route's meaning or weaken a hard constraint;
- its required evidence/authorization gates cannot be expressed as a provider-specific refinement of an existing route; and
- at least one concrete task case demonstrates why the new semantic category is needed.

If the distinction is only product/vendor/version/install-path specific, keep it in the adapter layer.

## New-route change contract

When adding a new route, update the portable contract consistently:

1. `SKILL.md` — add the route ID, portable name and selection meaning.
2. `SKILL.zh-tw.md` — keep the Traditional Chinese companion semantically aligned.
3. `references/routing-matrix.md` and `.zh-tw.md` — define required gates, authorization gates, pass evidence, failure meaning and stop conditions.
4. `references/route-contract.json` — add the machine-readable route metadata; increment `schema_version` only when the JSON contract shape or interpretation changes, not merely because one route row is added.
5. `examples/route-cases.md` and `.zh-tw.md` — add at least one positive selection case plus an ambiguity/failure or non-selection case.
6. `README.md` and `README.zh-tw.md` — update the visual route map and any C-range references.
7. Validate that route IDs, names and gate semantics agree across all maintained files, and that no new wording implies executable browser automation.

Route IDs are stable compatibility identifiers. Do not renumber existing classes to insert a new one; append a new ID unless a separately governed breaking migration is explicitly intended.

## Acceptance checklist

Before treating an extension as ready:

- The route/binding choice is explained: **why an existing route is enough, or why it is not**.
- Environment-specific facts remain in the adapter/provider binding rather than leaking into portable route semantics.
- Read-only probes are bounded and do not mutate state to manufacture a pass.
- Authority, capability, binding, execution and application outcome remain separate propositions.
- Explicit surface/profile/visibility/data-boundary constraints survive fallback.
- Synthetic examples cover at least one expected selection and one boundary case.
- English and Traditional Chinese maintained documents remain semantically aligned.

[Traditional Chinese companion](extending-routes.zh-tw.md)
