# Browser Tool Compass

**瀏覽器工具羅盤** · [繁體中文](README.zh-tw.md)

A portable decision skill for choosing the narrowest authorized browser tool.
It separates tool availability, user authority and application outcomes, so a
successful connection does not become an unsupported claim about profile access
or completed work.

The skill identifier remains `browser-tool-router` for compatibility with existing references.

## Contents

- [Skill entrypoint](SKILL.md): C1–C11 routing and fallback decisions.
- [Routing matrix](references/routing-matrix.md): read-only probe contracts, pass signals and stop conditions.
- [Adapter and evidence contract](references/adapter-evidence-contract.md): requirements for environment-specific tool bindings and scoped evidence.
- [Machine-readable route contract](references/route-contract.json): offline semantic metadata, not an executable router.
- [Synthetic examples](examples/route-cases.md): sixteen illustrative route decisions, not runtime results.

[Traditional Chinese overview](README.zh-tw.md) and [skill companion](SKILL.zh-tw.md)
are included; English is authoritative.

## Start here

For agent users and adapter authors:

1. Read [the decision steps](SKILL.md#decide-before-dispatch) and record the task's required surface, profile, visibility and allowed operations.
2. Choose one of the C1–C11 route classes and inspect its evidence requirements in the [routing matrix](references/routing-matrix.md).
3. Have your authorized environment adapter supply current, scoped evidence. Record missing evidence as `UNKNOWN`; a failed required gate stops that route.
4. Use the [decision record](SKILL.md#decision-record) to document the selection or blocker before handing an authorized action to the chosen tool.

The result is a documented route decision. The selected tool supplies browser
execution, and the task needs its own outcome verification.

## Provider-specific guidance

The contract includes an `openai-chatgpt-browser` binding under C6 for an
authorized existing-user Chrome integration. It requires current capability,
profile binding, exclusive profile control and applicable provider confirmation.
Extension installation alone does not establish availability. Its transport
remains opaque and unverified; see [the gates and fallback rules](SKILL.md#gates-fallback-and-stopping).

This independently maintained decision skill does not imply provider endorsement
or supply the named provider's runtime.

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

Distribution is limited to a private source repository. No license grant is
supplied; source rights and license terms must be resolved before public release
or redistribution. Host installation remains a separately authorized action.
