# Browser Tool Router

A portable decision skill for choosing the narrowest authorized browser tool.
It separates tool availability, user authority and application outcomes, so a
successful connection does not become an unsupported claim about profile access
or completed work.

## Contents

- [Skill entrypoint](SKILL.md): C1–C11 routing and fallback decisions.
- [Routing matrix](references/routing-matrix.md): read-only probe contracts, pass signals and stop conditions.
- [Adapter and evidence contract](references/adapter-evidence-contract.md): requirements for environment-specific tool bindings and scoped evidence.
- [Machine-readable route contract](references/route-contract.json): offline semantic metadata, not an executable router.
- [Synthetic examples](examples/route-cases.md): sixteen illustrative route decisions, not runtime results.

[Traditional Chinese overview](README.zh-tw.md) and [skill companion](SKILL.zh-tw.md)
are included; English is authoritative.

## Use and boundaries

Read the skill and the reference relevant to the task. An approved environment
adapter must supply exact available tools and probes before a route can be used.
The package itself has no service, database, mandatory framework or executable
browser adapter, and requires no build or package installation.

This is a local source repository, outside UniText registry coverage. It does
not install or project the skill into an agent host. No installer is supplied.
Live browser availability and acceptance are unverified. In particular,
installation, callable control and user-visible tabs remain separate gates.

## Validation and distribution

Use the host's skill validator, check that relative Markdown links resolve,
parse the route JSON and review behavior and translations. Static validation
cannot prove authenticated access, application success or absence of all
sensitive data. Local bootstrap decisions and validation receipts are excluded
from Git under `.local/`.

Distribution is local-only. No license grant is supplied; source rights and
license terms must be resolved before redistribution. Publication, remote push
and host installation are separate actions and are not performed by this skill.
