# Browser Tool Compass 瀏覽器工具羅盤

`openai-chatgpt-browser` 的具名 binding 定義於 route contract，使用 C6 處理已授權
的既有使用者瀏覽器互動。它不是通用 default、DevTools alias 或 isolated/remote
browser。選擇前須有當前 capability、availability、精確 profile/surface binding、
local policy/action authorization 與 profile control ownership；provider、Chrome、
OpenAI consent 是額外門檻，extension 安裝存在不能滿足這些要求。
沒有直接介面證據時維持 opaque transport、不推定 CDP 能力；static debugger call
不等於對外 CDP endpoint。Debugging 維持 C2、isolated remote automation 維持 C8、
低階既有 profile connection 維持 C7，不靜默改用 user browser。
重用 adapter 的 ownership/action taxonomy，區分 user、agent-owned、isolated、unknown
session；unknown binding 阻擋。同 profile 的 OpenAI、DevTools、direct-CDP 不得並行，
不同 tab 不證明 ownership 分離。Dispatch 前更新 ownership 證據；靜態 Skill 不取得
lock，也不證明 runtime acceptance。

本文件是 [SKILL.md](SKILL.md) 的繁體中文配套；英文版為權威版本，不是第二個技能入口。

選擇能產生所需證據、範圍最小且已獲授權的路由。本技能提供可攜式決策層與語意關卡；實際工具綁定及唯讀探測由另行核准的環境介接層提供。缺少核准介接層或當前成功探測時，執行可用性維持 `UNKNOWN`；若直接證據已確認失敗或阻擋，則分別記為 `FAIL` 或 `BLOCKED`。

## 派送前決策

1. 確認任務與不可放寬的條件：指定工具／介面、目標分頁、設定檔／工作階段、可見性、本機／遠端資料邊界及允許操作。不得為取得通過結果而放寬條件。
2. 先套用當前指令層級與授權。工具可用或探測通過都不產生操作權限。
3. 依下表選路由，只讀取[路由矩陣](references/routing-matrix.zh-tw.md)對應列。綁定工具或判讀不確定證據時，讀取[介接與證據契約](references/adapter-evidence-contract.zh-tw.md)。
4. 只執行核准介接層中、任務範圍內的唯讀探測。各關卡須記錄當前任務／情境證據；工具探索不等於執行通過。
5. 必要關卡通過後，依所選工具自身規則派送。實際操作仍需適用授權；可用性不等於操作驗收。

| 類別 | 可攜式路由 | 適用需求 |
|---|---|---|
| C1 | 公開內容擷取 | 不需登入或渲染 UI 的公開資料 |
| C2 | 瀏覽器開發檢查 | 主控台、網路、效能、DOM 或無障礙證據 |
| C3 | 工作階段自動化 | 不需專案測試的一次性分頁、互動或截圖 |
| C4 | 專案自有瀏覽器測試 | 可重複回歸、CI、trace；須確認專案所有權 |
| C5 | 主應用程式整合的可見瀏覽器 | 明確指定主應用程式瀏覽器介面或使用者分頁 |
| C6 | 使用者設定檔整合 | 透過設定檔整合存取既有使用者分頁或擴充功能情境 |
| C7 | 既有已驗證瀏覽器連線 | 綁定已授權既有設定檔，執行登入狀態下的驗收 |
| C8 | 遠端瀏覽器服務 | 明確授權的遠端執行或遠端設定檔 |
| C9 | 本機瀏覽器 CLI 冒煙檢查 | 對已可用開發目標進行視覺檢查 |
| C10 | 桌面互動 | 桌面 UI，或瀏覽器 API 無法完成需求時的已授權最後替代方案 |
| C11 | 擴充功能專案骨架 | 建立擴充功能或相關治理；執行驗證另用其他路由 |

登入狀態驗收通常選 C7；若指定整合介面，則可選 C6。兩者都不自行授權同意畫面或寫入操作。C6／C7 須證明當前連線綁定指定設定檔／工作階段，僅傳輸正常不足。回送位址或檔案目標本身不強制 C5，仍依介面與證據需求選擇。

## 關卡、替代與停止

- C5 的 `installed`、`runnable`、`visible` 須分開通過。`visible` 表示受控的指定分頁確實出現在要求的使用者介面；隱藏控制不足。
- 關卡失敗即停止該路由。替代方案須保留全部硬性條件並符合授權，使用前說明路由變更與證據限制。不得默換指定介面／設定檔；沒有合格替代時回報 `BLOCKED`。
- C8 不得自動替代本機工具、檔案、服務或使用者設定檔；遠端處理需明確範圍與資料邊界檢查。
- 不得為通過關卡而安裝工具、啟動／重啟服務、啟用擴充功能、改設定或信任控制、編輯快取、複製設定檔。應回報邊界及另行治理的後續動作。不得繞過登入、同意、付費牆、安全警告、CAPTCHA、密碼管理員或使用者確認。
- 不讀取或記錄 cookies、權杖、憑證、工作階段儲存內容及敏感頁面文字作為探測證據；僅保留最少、已去敏的中繼資料與不透明識別碼。
- 工具／執行環境、連線、設定檔、目標或可見性變更後，更新受影響證據。歷史報告或靜態檔案只能協助探索，不能證明當前執行成功。
- 傳輸、設定檔綁定、可見性、應用程式行為分開記錄。應用操作失敗不抹除已證明的傳輸通過，也不能宣稱應用驗收通過。

## 決策紀錄

記錄任務與硬性條件、授權與允許操作、主要路由與介接層身分、各關卡結果與去敏證據情境、替代資格或無法替代原因、敏感檢查點及預期成果證據。

關卡狀態使用 `PASS`、`FAIL`、`BLOCKED`、`UNKNOWN`、`NOT_APPLICABLE`；事實分類另用 `VERIFIED`、`INFERRED`、`UNKNOWN`。推論綁定不算通過綁定。模糊情境見[合成案例](examples/route-cases.zh-tw.md)；[精簡路由契約](references/route-contract.json)只供離線一致性檢查，不是瀏覽器實作或執行證據。
