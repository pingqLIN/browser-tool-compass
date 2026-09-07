# 瀏覽器工具羅盤

**Browser Tool Compass** · [English](README.md)

![貓頭鷹導航員手持黃銅羅盤，指向穿越幾何節點的青綠色路徑。](docs/assets/readme/browser-tool-compass-owl-banner.png)

這個可攜式決策技能協助選擇範圍最小且已獲授權的瀏覽器工具。它分開記錄工具可用性、使用者授權與應用成果，避免把連線成功誤認為可存取指定設定檔，或整個任務已完成。

技能識別碼維持 `browser-tool-router`，以相容既有引用。

## 內容

- [技能入口](SKILL.md)與[繁中配套](SKILL.zh-tw.md)：C1–C11 路由及替代決策。
- [路由矩陣](references/routing-matrix.zh-tw.md)：唯讀探測契約、通過訊號與停止條件。
- [介接與證據契約](references/adapter-evidence-contract.zh-tw.md)：環境工具綁定與範圍內證據的要求。
- [機器可讀路由契約](references/route-contract.json)：離線語意資料，不是可執行路由器。
- [合成案例](examples/route-cases.zh-tw.md)：16 個示範決策，不是實測結果。

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

## 驗證與散布

使用 host 提供的技能驗證器，檢查 Markdown 相對連結、解析路由 JSON，並審查決策行為與雙語語意。靜態驗證不能證明登入態存取、應用成功或完全沒有敏感資料。本機 bootstrap 決策與驗證收據存於 Git 忽略的 `.local/`。

目前以私人來源倉庫存放，未提供授權條款；公開發布或再散布前須確認來源權利及 license。Host 安裝仍須另外取得授權。
