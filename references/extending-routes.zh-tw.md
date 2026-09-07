# Agent 擴充契約：路由與環境綁定

本文件的主要讀者是維護或調整 Browser Tool Compass 的 **AI Agent、Lead Agent 與 workflow orchestrator**，不是提供人類逐步手動操作的 setup guide。

Browser Tool Compass 將**可攜式路由語意**與**環境特定工具綁定**分開。預設規則是：

> **先把新工具映射到既有 C1–C11 route。只有既有 taxonomy 無法保留任務的 surface、authority、evidence、binding、visibility 或 data-boundary 語意時，才新增 route class。**

新增 product、plugin、MCP tool、CLI、browser integration 或 provider，**不等於**就要新增 route。

## Agent 分類契約

當 Agent 遇到目前環境尚未表示的 browser-related capability，在編輯專案檔案前，MUST 先分類這次維護變更。

### `BINDING_EXTENSION`

既有 C1–C11 已能表示所需任務語意時使用。

Agent SHOULD 保留原 C-class，只新增或更新 environment adapter / provider binding。必須識別精確工具、可信任指引、有限範圍的 read-only probe、gate semantics、profile/session binding 要求、visibility constraint、data boundary 與 failure behavior。

### `PROVIDER_REFINEMENT`

既有 route 語意正確，但某個 provider 額外需要 runtime、consent、ownership、profile-binding 或其他 gate 時使用。

Agent SHOULD 保留 route ID，只加入該環境真正需要的 provider-specific binding/gates。`references/route-contract.json` 中的 provider metadata 仍只是離線語意 metadata，不會因此讓 provider 變成可呼叫工具。

### `NEW_ROUTE_PROPOSAL`

只有以下條件全部成立時才使用：

- 所需 browser surface 或 execution boundary 與每一個既有 C-class 都有實質差異；
- 若套用既有 route，會改變其語意或削弱 hard constraint；
- 所需 evidence/authorization gates 無法只透過 existing route 的 provider refinement 表達；
- 至少有一個具體 task case 能證明確實需要新的 semantic category。

如果差異只是 product/vendor/version/install-path，Agent MUST NOT 新增 route。不要因為環境剛好出現第十二個 browser product，就建立 C12。

## Agent 必須先產生的判斷紀錄

在修改 taxonomy 或 binding 前，先產生精簡的 maintenance record：

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

這是 maintenance decision artifact，不是 runtime evidence。

## Existing-route binding 契約

對 `BINDING_EXTENSION` 或 `PROVIDER_REFINEMENT`，Agent MUST 保留 portable route semantics，除非已有證據證明既有語意不足。

產出的 binding 應定義：

- adapter identity/version 與 supported environment；
- 支援 route IDs 與明確不支援的 routes；
- 目前實際暴露的精確工具與 trusted tool instructions；
- path/tool name 可能變動時的 discovery rules；
- bounded read-only probes，以及每個 probe 實際證明的 proposition；
- expected response fields 與 `PASS` / `FAIL` / `BLOCKED` / `UNKNOWN` / `NOT_APPLICABLE` 解讀；
- 必要時的 identity/profile/session evidence；
- local/remote data-boundary rules 與最小回傳 metadata；
- stop、cleanup 與 ownership behavior。

Agent MUST NOT 為了製造 probe PASS 而自行安裝、啟動、修復、重新設定工具或降低 trust controls。

## New-route 變更契約

對 `NEW_ROUTE_PROPOSAL`，Agent MUST 一致地更新 portable contract，而不是只修改單一檔案：

1. `SKILL.md` — route ID、portable name 與 selection semantics。
2. `SKILL.zh-tw.md` — 與英文保持語意一致。
3. `references/routing-matrix.md` 與 `.zh-tw.md` — required gates、authorization gates、pass evidence、failure meaning 與 stop conditions。
4. `references/route-contract.json` — machine-readable route metadata。只有 JSON contract shape 或 interpretation 改變時才增加 `schema_version`，不是只因新增一列 route。
5. `examples/route-cases.md` 與 `.zh-tw.md` — 至少一個 positive selection case，以及一個 ambiguity/failure/non-selection case。
6. `README.md` 與 `README.zh-tw.md` — Agent route map 與所有 C-range references。
7. 重新檢查 route IDs、names、gate semantics 的一致性，並確認任何新增文字都沒有暗示本專案是可執行的 browser automation runtime。

Route ID 是穩定的 compatibility identifier。Agent MUST NOT 為了插入新類別就重新編號既有 classes；除非已明確授權一個獨立治理的 breaking migration，否則追加下一個 ID。

## 驗收條件

Maintaining Agent 只有在以下條件都成立時，才可把 extension 標記為 ready：

- binding-vs-route 分類明確且有理由；
- environment-specific facts 沒有滲入 portable route semantics，除非它真的構成新的 semantic boundary；
- probes 有界且不會透過 mutation 製造 PASS；
- authority、capability、binding、execution 與 application outcome 仍是分離的 proposition；
- explicit surface/profile/visibility/data-boundary constraints 在 fallback 後仍保留；
- synthetic examples 涵蓋預期選擇與至少一個 boundary case；
- 英文與繁中維護文件語意一致；
- repository 仍清楚描述 decision/router skill，而不是 browser automation runtime。

[English companion](extending-routes.md)
