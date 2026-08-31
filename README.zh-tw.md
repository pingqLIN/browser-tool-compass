# Browser Tool Router

這個可攜式決策技能協助選擇範圍最小且已獲授權的瀏覽器工具。它分開記錄工具可用性、使用者授權與應用成果，避免把連線成功誤認為可存取指定設定檔，或整個任務已完成。

## 內容

- [技能入口](SKILL.md)與[繁中配套](SKILL.zh-tw.md)：C1–C11 路由及替代決策。
- [路由矩陣](references/routing-matrix.zh-tw.md)：唯讀探測契約、通過訊號與停止條件。
- [介接與證據契約](references/adapter-evidence-contract.zh-tw.md)：環境工具綁定與範圍內證據的要求。
- [機器可讀路由契約](references/route-contract.json)：離線語意資料，不是可執行路由器。
- [合成案例](examples/route-cases.zh-tw.md)：16 個示範決策，不是實測結果。

[英文版](README.md)與英文技能文件為權威版本。

## 使用與邊界

先讀技能及任務相關參照。使用路由前，須由已核准的環境 adapter 提供精確可用工具與探測方式。本包沒有服務、資料庫、必要 framework 或可執行瀏覽器 adapter，不需要建置或安裝套件。

本專案是本機來源倉庫，不納入 UniText registry，也不會將技能安裝或投影至 agent host；未提供 installer。瀏覽器即時可用性與驗收尚未證實，安裝、可呼叫控制及使用者可見分頁仍是不同關卡。

## 驗證與散布

使用 host 提供的技能驗證器，檢查 Markdown 相對連結、解析路由 JSON，並審查決策行為與雙語語意。靜態驗證不能證明登入態存取、應用成功或完全沒有敏感資料。本機 bootstrap 決策與驗證收據存於 Git 忽略的 `.local/`。

目前僅供本機使用，未提供授權條款；散布前須確認來源權利及 license。發布、remote push 與 host 安裝是獨立動作，本技能不會執行這些操作。
