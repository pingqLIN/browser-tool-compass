# Synthetic route cases

All cases are synthetic teaching fixtures, not execution receipts. Assume no runtime pass unless explicitly supplied in the case. These examples explain decisions; independent validation should use additional cases.

| Case | Facts and request | Correct decision |
|---|---|---|
| Public content | Retrieve a public guide without rendered interaction. | C1. Do not open an authenticated profile. A successful response proves retrieval only. |
| Inspect signals | Diagnose console errors on an authorized target; C2 exposes target metadata and console capability. | C2 connection may pass. Capture console contents only under the actual inspection scope and sanitization rules. |
| One-off action | Take a screenshot using any approved local browser; C3 lists no tabs. | Session transport can pass; target readiness has not. Create/navigate a target only as a separately authorized action within the task. |
| Repeatable tests | Add browser regression to the named project; a different directory contains an unrelated scaffold. | Select C4, verify intended project ownership. Do not reuse unrelated scaffold or install dependencies in the availability probe. |
| Hidden host browser | “Use the visible browser in this host app.” Installation and control pass; intended tab is hidden. | C5-visible fails; task is blocked on the required surface. C3 screenshots cannot satisfy it. |
| Profile integration | Inspect the existing user's extension panel through the profile integration. Runtime responds, but binding is absent. | Select C6; profile binding is `UNKNOWN`, so stop. A familiar page title cannot replace identity evidence. |
| Authenticated connection | Validate the current signed-in service session. Existing endpoint handshake succeeds; target is a login page. | C7 connection can pass, authenticated binding does not. Stop; do not login, copy cookies or switch to a fresh browser. |
| Stale authentication | Prior task verified the same endpoint, but this task has no profile evidence. | C7 current readiness is `UNKNOWN`; refresh required binding without reading secrets. |
| Remote temptation | Local route unavailable; remote service works; task is local-only. | C8 is `BLOCKED` by scope. Do not upload local page content or treat remote success as local acceptance. |
| CLI readiness | Approved CLI responds to a non-mutating availability check; development target is down. | C9-cli_available passes; target_ready fails or is blocked. Do not start the server merely to pass preflight. |
| Desktop fallback | Browser APIs unavailable; user requires network timing evidence. Desktop tool works. | C10 cannot satisfy the evidence requirement. Report blocked instead of replacing timing evidence with a screenshot. |
| Extension target | Request a new extension project in a directory containing work of unknown ownership. | C11 scaffold_scope is blocked. Do not overwrite, delete or relocate existing files. |
| App interaction failure | C3 target listing passed; authorized click fails because a control is covered. | Transport remains `PASS`; interaction is `FAIL`; the flow remains unproven. Do not repair browser trust or configuration. |
| Trust rejection | C5 runtime connection is rejected by trust policy; C5 installation exists. | Runnable is `BLOCKED`; installation remains `PASS`. Do not change allowlists or borrow another runtime to evade the rejection. |
| Changed context | C6 runtime passed, then the integration reconnects to another profile. | Invalidate affected runtime/binding evidence and recheck the authorized identity before any operation. |
| Remote receipt | Authorized remote operation returns “queued.” | Record submission/queue state only; execution and application outcome stay `UNKNOWN`. |

[Traditional Chinese companion](route-cases.zh-tw.md)
