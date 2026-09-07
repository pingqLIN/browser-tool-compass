# 擴充路由與環境綁定

Browser Tool Compass 會將**可攜式路由語意**與**環境特定工具綁定**分開。大多數環境應擴充 adapter 層，而不是直接擴充 C-class 分類。

## 第一個判斷：新增 binding，還是新增 route？

只要新的瀏覽器工具能在不削弱任務限制、授權、證據、profile/session 綁定、可見性或 data boundary 的前提下，符合既有 C1–C11 任一路由的語意，就應優先沿用既有 route。

**新增一個工具，不代表就要新增一條 route。**

| 情況 | 建議修改方式 |
|---|---|
| 新產品／新工具實作的任務語意與既有 C-class 相同 | 新增或更新 environment adapter / provider binding |
| Host 只是透過不同 command、plugin、MCP tool 或安裝路徑暴露相同 route | 更新 environment adapter |
| Provider 額外需要 runtime、consent、ownership 或 profile-binding 檢查 | 在既有 route 上補 provider-specific gate/binding；只要核心語意仍相同，就不要新增 route |
| 既有任何 C-class 都無法表示新的 browser surface 或 execution boundary，除非改變原本語意 | 才提出新的 route class |

不要因為某個環境剛好有第十二個瀏覽器產品，就直接建立 C12。

## 將新工具加入既有 route

1. **先選語意 route。** 依 `SKILL.md` 與 `references/routing-matrix.md` 的 task surface 和 execution boundary，將工具對應到 C1–C11。
2. **定義 environment binding。** 記錄 adapter identity/version、支援環境、實際暴露工具、route IDs、不支援 route，以及可信任的工具指引來源。
3. **定義唯讀 probe。** 說明精確 probe、可能副作用、要驗證的 proposition，以及哪些回應欄位構成 `PASS`、`FAIL`、`BLOCKED`、`UNKNOWN`。Probe 不得為了通過關卡而安裝、啟動、修復或重新設定工具。
4. **授權與證據保持分離。** 工具可用不等於取得 action authority。需要時，profile/session binding、visibility、remote data handling 與 provider consent 都必須維持為獨立 gate。
5. **測試模糊與失敗情況。** 缺少證據時應維持 `UNKNOWN`；required gate 失敗必須停止該 route；fallback 不得偷偷改變明確指定的 surface/profile/data-boundary 限制。

若對離線一致性檢查有幫助，可以在 `references/route-contract.json` 的 `provider_bindings` 中記錄 provider-specific metadata。該項目只是語意 metadata，**不會因此讓 provider 變成可呼叫工具**。

## 什麼情況才值得新增 route class

只有在以下條件全部成立時，才建議建立新 route：

- 任務需要的 surface 或 execution boundary 與所有既有 C-class 都有實質差異；
- 若硬套既有 route，會改變該 route 的語意或削弱 hard constraint；
- 新的 required evidence / authorization gates 無法只透過 existing route 的 provider-specific refinement 表達；
- 至少有一個具體 task case 能說明為什麼需要新的語意類別。

如果差異只是 product/vendor/version/install-path，應保留在 adapter 層。

## 新 route 的變更契約

新增 route 時，需要同步更新可攜式契約：

1. `SKILL.md` — 新增 route ID、可攜式名稱與選擇語意。
2. `SKILL.zh-tw.md` — 維持繁中配套文件語意一致。
3. `references/routing-matrix.md` 與 `.zh-tw.md` — 定義 required gates、authorization gates、pass evidence、failure meaning 與 stop conditions。
4. `references/route-contract.json` — 新增 machine-readable route metadata；只有 JSON contract 的結構或解讀方式改變時才增加 `schema_version`，不是只因為多一列 route 就增加。
5. `examples/route-cases.md` 與 `.zh-tw.md` — 至少補一個正向選擇案例，以及一個 ambiguity/failure 或不應選擇該 route 的案例。
6. `README.md` 與 `README.zh-tw.md` — 更新 visual route map 與所有 C-range 描述。
7. 驗證所有維護檔案中的 route ID、名稱與 gate semantics 一致，而且新增文案不能讓人誤以為本專案具有可執行 browser automation 能力。

Route ID 是穩定的 compatibility identifier。不要為了插入新類別而重新編號既有 class；除非另有明確治理的 breaking migration，否則應新增下一個 ID。

## 驗收清單

擴充完成前至少確認：

- 已說明為何沿用 existing route 即可，或為何 existing route 確實不足。
- Environment-specific facts 留在 adapter/provider binding，沒有滲入 portable route semantics。
- Read-only probe 有明確邊界，而且不會透過改變狀態製造 PASS。
- Authority、capability、binding、execution 與 application outcome 仍是不同 proposition。
- Explicit surface/profile/visibility/data-boundary constraints 在 fallback 後仍被保留。
- Synthetic examples 至少涵蓋一個預期選擇案例與一個 boundary case。
- 英文與繁中維護文件保持語意一致。

[English companion](extending-routes.md)
