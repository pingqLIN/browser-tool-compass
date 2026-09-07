# Browser Tool Compass

**瀏覽器工具羅盤** · [繁體中文](README.zh-tw.md)

> **Version: v0.1.0 (manual)** · Private source repository · No automated release pipeline

![An owl navigator holding a brass compass points toward a teal route through geometric waypoints.](docs/assets/readme/browser-tool-compass-owl-banner.png)

**Browser Tool Compass is a decision/router skill for AI agents. It decides which browser-related tool route is appropriate, authorized, and sufficiently evidenced before execution is handed to that tool.**

It is **not browser automation**. It does not click pages, drive tabs, log in, install extensions, launch browsers, or claim that a downstream action succeeded. Its output is a **route decision plus evidence gates**; the selected browser tool performs the actual operation and the task must verify the result separately.

The skill identifier remains `browser-tool-router` for compatibility with existing references.

## Mental model

```text
Task request
    ↓
Browser Tool Compass
    ├─ What browser surface is actually required?
    ├─ Which tool route fits that requirement?
    ├─ Is the user/agent authorized to use it?
    └─ Is there current evidence that the route is available and correctly bound?
    ↓
Route decision: C1–C11 + PASS / FAIL / BLOCKED / UNKNOWN gates
    ↓
Selected browser tool executes the action
    ↓
Task-specific outcome verification
```

In short: **Compass chooses and justifies the route; it does not execute the route.**

## What it does — and does not do

| Browser Tool Compass does | Browser Tool Compass does not |
|---|---|
| Classify a browser task into one of C1–C11 routes | Operate a browser or webpage |
| Check authority, profile/session binding, visibility and evidence requirements | Grant authority because a tool exists or connects |
| Record `PASS`, `FAIL`, `BLOCKED`, `UNKNOWN` and fallback eligibility | Treat installation or transport success as proof of usable browser control |
| Produce a documented route decision before dispatch | Verify that the downstream browser action completed successfully |
| Define portable semantics that environment adapters can implement | Bundle a browser runtime, service, database or executable adapter |

## Who this is for

- **AI agent / workflow authors** who need a deterministic way to choose among browser-related tools.
- **Environment adapter authors** who map portable route semantics to the tools and probes available in a particular host.
- **Governance or review workflows** that need to distinguish capability, authority, binding, execution and outcome evidence.

If your goal is simply to automate a webpage, this repository is not the automation engine. Use the chosen browser tool itself after the route and authorization gates pass.

## Example

Suppose an agent is asked to interact with an already authenticated tab in the user's existing Chrome profile.

Browser Tool Compass does **not** immediately launch a browser controller. It first determines whether the task requires a user-profile integration (for example C6) or another route, then requires current evidence for the intended profile/session, control ownership, allowed operations and provider-specific gates. If those conditions do not pass, the route is reported as `BLOCKED` or `UNKNOWN` rather than silently substituting another browser environment.

Only after the route passes does execution move to the selected browser tool. That tool's result still requires separate task-level verification.

## Contents

- [Skill entrypoint](SKILL.md): C1–C11 routing and fallback decisions.
- [Routing matrix](references/routing-matrix.md): read-only probe contracts, pass signals and stop conditions.
- [Adapter and evidence contract](references/adapter-evidence-contract.md): requirements for environment-specific tool bindings and scoped evidence.
- [Machine-readable route contract](references/route-contract.json): offline semantic metadata, **not an executable router**.
- [Synthetic examples](examples/route-cases.md): sixteen illustrative route decisions, **not runtime results**.

[Traditional Chinese overview](README.zh-tw.md) and [skill companion](SKILL.zh-tw.md) are included; English is authoritative.

## Start here

For agent users and adapter authors:

1. Read [the decision steps](SKILL.md#decide-before-dispatch) and record the task's required surface, profile, visibility and allowed operations.
2. Choose one of the C1–C11 route classes and inspect its evidence requirements in the [routing matrix](references/routing-matrix.md).
3. Have your authorized environment adapter supply current, scoped evidence. Record missing evidence as `UNKNOWN`; a failed required gate stops that route.
4. Use the [decision record](SKILL.md#decision-record) to document the selection or blocker before handing an authorized action to the chosen tool.

The result is a documented route decision. The selected tool supplies browser execution, and the task needs its own outcome verification.

## Provider-specific guidance

The contract includes an `openai-chatgpt-browser` binding under C6 for an authorized existing-user Chrome integration. It requires current capability, profile binding, exclusive profile control and applicable provider confirmation. Extension installation alone does not establish availability. Its transport remains opaque and unverified; see [the gates and fallback rules](SKILL.md#gates-fallback-and-stopping).

This independently maintained decision skill does not imply provider endorsement or supply the named provider's runtime.

## Use and boundaries

Read the skill and the reference relevant to the task. An approved environment adapter must supply exact available tools and probes before a route can be used. The package itself has no service, database, mandatory framework or executable browser adapter, and requires no build or package installation.

This is a local source repository, outside UniText registry coverage. It does not install or project the skill into an agent host. No installer is supplied. Live browser availability and acceptance are unverified. In particular, installation, callable control and user-visible tabs remain separate gates.

## Versioning

Current project version: **v0.1.0**.

Versioning is currently **manual**: the maintained version number is written explicitly in the README and is not evidence of an automated GitHub Release, package publication or deployment. Until a release process is introduced, version changes should be made intentionally together with the corresponding source/documentation update and commit.

## Validation and distribution

Use the host's skill validator, check that relative Markdown links resolve, parse the route JSON and review behavior and translations. Static validation cannot prove authenticated access, application success or absence of all sensitive data. Local bootstrap decisions and validation receipts are excluded from Git under `.local/`.

Distribution is limited to a private source repository. No license grant is supplied; source rights and license terms must be resolved before public release or redistribution. Host installation remains a separately authorized action.
