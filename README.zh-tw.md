# 瀏覽器工具羅盤

**Browser Tool Compass** · [English](README.md)

> **版本：v0.1.0（手動標註）** · 私人來源倉庫 · 尚無自動化 release pipeline

![貓頭鷹導航員手持黃銅羅盤，指向穿越幾何節點的青綠色路徑。](docs/assets/readme/browser-tool-compass-owl-banner.png)

**Browser Tool Compass 是給 AI agent 使用的 browser-tool decision/router skill。它的工作，是在真正執行瀏覽器操作之前，判斷哪一條瀏覽器工具路徑最適合、已獲授權，而且有足夠的當前證據支持。**

它**不是 browser automation 工具**。它不負責點擊頁面、操作分頁、登入網站、安裝擴充功能、啟動瀏覽器，也不會因為工具連線成功就宣稱下游操作已完成。它的產出是**路由決策與證據關卡**；真正的瀏覽器操作由選定工具負責，任務成果仍須另外驗證。

技能識別碼維持 `browser-tool-router`，以相容既有引用。

## 心智模型

```text
任務需求
    ↓
Browser Tool Compass
    ├─ 真正需要哪一種瀏覽器操作介面／surface？
    ├─ 哪一條工具路由最符合需求？
    ├─ 使用者／Agent 是否有權限使用？
    └─ 是否有當前證據證明該路由可用，且綁定到正確 profile/session？
    ↓
路由決策：C1–C11 + PASS / FAIL / BLOCKED / UNKNOWN 關卡
    ↓
選定的瀏覽器工具實際執行
    ↓
任務層級的結果驗證
```

簡單說：**Compass 負責選路與說明理由，不負責實際走那條路。**

## 它會做什麼，以及不會做什麼

| Browser Tool Compass 會做 | Browser Tool Compass 不會做 |
|---|---|
| 將瀏覽器任務分類到 C1–C11 路由 | 直接操作瀏覽器或網頁 |
| 檢查授權、profile/session 綁定、可見性與證據要求 | 因為工具存在或連得上，就自動視為已獲授權 |
| 記錄 `PASS`、`FAIL`、`BLOCKED`、`UNKNOWN` 與 fallback 資格 | 把安裝成功或 transport 成功當作「可控制使用者瀏覽器」的證明 |
| 在 dispatch 前產生有依據的路由決策 | 驗證下游瀏覽器操作是否真的完成 |
| 定義可攜式語意，讓不同環境 adapter 實作 | 內建瀏覽器 runtime、服務、資料庫或可執行 adapter |

## 適合誰使用

- **AI agent / workflow 作者**：需要一套穩定方式，在多種瀏覽器工具之間做選擇。
- **環境 adapter 作者**：需要把可攜式路由語意對應到某個 host 中真正可用的工具與 probe。
- **治理／審查流程**：需要分清楚 capability、authority、binding、execution 與 outcome evidence。

如果你的目標只是「自動操作一個網頁」，這個專案不是 automation engine。應先讓路由與授權關卡通過，再交給真正的瀏覽器工具執行。

## 範例

假設 Agent 被要求操作「使用者現有 Chrome profile 中，已登入的分頁」。

Browser Tool Compass 不會直接啟動 browser controller，而是先判斷任務應使用 user-profile integration（例如 C6）或其他路由，再要求取得當前的 profile/session 綁定、控制權、允許操作與 provider-specific gate 證據。如果條件不成立，就回報 `BLOCKED` 或 `UNKNOWN`，而不是偷偷改用另一個瀏覽器環境。

只有在路由關卡通過後，才把執行交給選定的瀏覽器工具；而該工具回傳的結果，仍需在任務層級另外驗證。

## 內容

- [技能入口](SKILL.md)與[繁中配套](SKILL.zh-tw.md)：C1–C11 路由及替代決策。
- [路由矩陣](references/routing-matrix.zh-tw.md)：唯讀探測契約、通過訊號與停止條件。
- [介接與證據契約](references/adapter-evidence-contract.zh-tw.md)：環境工具綁定與範圍內證據的要求。
- [機器可讀路由契約](references/route-contract.json)：離線語意資料，**不是可執行路由器**。
- [合成案例](examples/route-cases.zh-tw.md)：16 個示範決策，**不是實測結果**。

[英文版](README.md)與英文技能文件為權威版本。

## 從這裡開始

供 agent 使用者與 adapter 作者使用：

1. 閱讀[決策步驟](SKILL.md#decide-before-dispatch)，記錄任務要求的操作介面、設定檔、可見性及允許的操作。
2. 選擇 C1–C11 中的路由類別，並在[路由矩陣](references/routing-matrix.zh-tw.md)確認所需證據。
3. 由已獲授權的環境 adapter 提供當前且符合範圍的證據。缺少證據時記錄為 `UNKNOWN`；必要關卡失敗時停止該路由。
4. 使用[決策記錄](SKILL.md#decision-record)整理選擇結果或阻擋原因，再將已獲授權的操作交給選定工具。

產出是一份有依據的路由決策。選定工具負責執行瀏覽器操作，任務成果仍須另外驗證。

## 特定 provider 的使用指引

契約將 `openai-chatgpt-browser` 綁定至 C6，適用於已獲授權的既有使用者 Chrome 整合。使用前須確認當前能力、設定檔綁定、設定檔控制權不衝突，以及適用的 provider 確認。僅安裝擴充功能不能證明工具可用；其傳輸方式仍為 opaque，尚未驗證。詳見[關卡與替代路由規則](SKILL.md#gates-fallback-and-stopping)。

本決策技能獨立維護，不代表獲得 provider 背書，也不提供該 provider 的 runtime。

## 使用與邊界

先讀技能及任務相關參照。使用路由前，須由已核准的環境 adapter 提供精確可用工具與探測方式。本包沒有服務、資料庫、必要 framework 或可執行瀏覽器 adapter，不需要建置或安裝套件。

本專案是本機來源倉庫，不納入 UniText registry，也不會將技能安裝或投影至 agent host；未提供 installer。瀏覽器即時可用性與驗收尚未證實，安裝、可呼叫控制及使用者可見分頁仍是不同關卡。

## 版本管理

目前專案版本：**v0.1.0**。

目前採**手動版本標註**：版本號明確寫在 README 中，但它不代表已有自動化 GitHub Release、套件發布或部署流程。直到正式建立 release 流程以前，版本號應在每次有意義的來源／文件更新時，由維護者主動調整，並與對應 commit 一起提交。

## 驗證與散布

使用 host 提供的技能驗證器，檢查 Markdown 相對連結、解析路由 JSON，並審查決策行為與雙語語意。靜態驗證不能證明登入態存取、應用成功或完全沒有敏感資料。本機 bootstrap 決策與驗證收據存於 Git 忽略的 `.local/`。

目前以私人來源倉庫存放，未提供授權條款；公開發布或再散布前須確認來源權利及 license。Host 安裝仍須另外取得授權。
