# 狀態面板 variableViewHtml
> 高度、`<style>` 無效、bytes 上限是官方規則；標籤、屬性、CSS 變數與模板語法來自早期官方說明，工具報錯時以報錯為準；圖示格子與內層捲動是實測做法。
- MCP 欄位：`variableViewMode`（`default` 基本模式、`preset` 預設集、`custom` 自訂 HTML）、`variableViewPreset`、`variableViewHtml`。
- 上限 10,240 bytes（UTF-8）。只能 inline style 與官方 CSS 變數；不能 `<script>`、外部 CSS／JS，style 內不能用 `expression()`、`url()`、`@import`。
- 允許標籤：h1～h6、p、br、hr、strong、em、b、i、u、s、del、mark、small、sub、sup、ul、ol、li、dl、dt、dd、a、img、figure、figcaption、blockquote、pre、code、table 系列、div、span、section、article、details、summary、video、source。
- 允許屬性：href、target、rel、src、alt、width、height、class、id、style、colspan、rowspan、scope、controls、autoplay、loop、muted、playsinline、open、loading。沒有 input、label、button，`details` 也沒有 `name`，所以不能做互斥展開，按鈕式面板只能各個 `details` 獨立展開。
- 文字、背景、邊框一律用 CSS 變數（--background、--foreground、--surface、--surface-elevated、--text-secondary、--text-label、--border-subtle、--border、--color-primary、--color-negative 等），才能同時支援深色與淺色模式。寫死的顏色只用在裝飾（例如角色色邊）。半透明用 `color-mix(in srgb, var(--foreground) N%, transparent)`。
- 模板語法：`{{變數名}}`、`{{變數名:numeric}}`（取出 0～100 的數字，用在 `width:N%`）、`{{user}}`、`{{charN}}`、`{{#each variables}}…{{/each}}`（內有 name、value、numericValue）。
- 條件：`{{#range 變數 min max}}`（min ≤ 值 < max）、`gt`、`gte`、`lt`、`lte`、`eq`、`neq`，比較全是數字。
- 數學：`{{math 變數 + 值}}`，由左到右計算，沒有運算優先序，可接 `| round`、`| floor`、`| ceil`。
- 格式：`{{repeat 字 變數 單位}}`、`{{gauge 變數 填滿 空格 格數}}`、`{{icon 變數 門檻:圖示 …}}`。
- 內建 CSS 類別可直接用：vv-rpg、vv-neon、vv-crystal、vv-dating、vv-pixel、vv-survival、vv-ribbon、vv-minimal、vv-gothic、vv-academy、vv-scifi。
- 面板可以用 emoji。
- 複合文字變數（含「｜」）整串顯示，條件與數學只吃數字；`:numeric` 會取到複合文字中的哪個數字未確認，要拆格就改成獨立的數字變數。
- 按鈕式面板：按鈕放外面、點開在原地展開，用 `details`／`summary` 做，做完請使用者在預覽確認效果。
- **狀態面板高度超過 300px 下方會被裁切，也不會出現捲軸（官方限制）**。卡片橫向排列（`flex-wrap`），不垂直堆疊。收合時就要留出展開空間：收合高度加上展開內容的最大高度要 ≤ 300px。常駐資訊只放一個緊湊卡片，其餘內容全部收進按鈕，展開內容的容器加 `max-height` 與 `overflow-y:auto`。能開瀏覽器的環境，寫完用本機頁面量收合與各種展開狀態的高度；其餘環境照行數、字級與內距算出高度，並請使用者在預覽確認。
- **首選：流內展開的圖示格子（已在本機驗證）**：展開內容在圖示列下方自然展開、再點收起，不是浮層。結構：
  - 最外層 `max-height:280px;overflow-y:auto`（Tikita 外層另有 300px 裁切，內層自己捲動，所以展開再多也不會被裁）。
  - 每一排是 `position:relative;padding-top:<圖示列高度>px` 的 div，padding-top 預留給圖示與標籤。
  - 每個項目一個 `<details style='display:block'>`；它的 `<summary>` 用 `position:absolute;top:0;left:N%;width:20%`（五格就是 0、20、40、60、80%），所以圖示全部疊在同一排，不佔流內高度。
  - 展開內容 div 放在 `details` 裡、在流內（固定高度加 `overflow-y:auto`、上下 margin 幾 px），展開時把這一排與其下所有東西往下推，收起就回到原高度。
  - 因為每個 details 獨立，可同時展開多個，會往下堆疊，由外層捲動承接。
  - 收合時的高度很矮（實測約 242px），展開時最高 280px 加內層捲動。
  - 深色面板在外層加 `color-scheme:dark;scrollbar-width:thin`，否則捲軸是亮白色。
  - 沒有固定中間顯示區，預設會留一行小字提示「點選圖示展開，再點一次收起」。
  - 圖示加徽章加名字這類由變數截取出的小元件，一樣放在 summary 裡（全形寬度視窗截取法照用）。
- 使用者提供別人的面板原始碼時，優先讀它的結構再套用，不要先斷定做不到。

## 官方補充
- 面板有基本模式（列出名稱與數值）與 HTML 模式。HTML 模式有 13 個預設集晶片（RPG 數值、霓虹光暈、水晶、好感度、像素復古、生存、緞帶優雅、極簡卡片、Emoji 量表、圖示氛圍、哥德深色、學院成績單、科幻全息圖）；按晶片會**附加**在現有程式碼下方，要替換先清空輸入欄，重疊按兩個會出現兩個面板。
- 畫面有「複製 AI 指南」按鈕，把指南貼給 ChatGPT 或 Claude 說明想要的樣式，再把產出貼回 HTML 欄。
- 官方配方的 HTML 結構：外層 `display:flex;flex-wrap:wrap;gap:6px;font-size:12px`，第一張卡 `flex:1 1 100%` 放日期時間，其餘卡 `flex:1 1 120px;min-width:0`，卡片 `padding:9px 12px;border-radius:9px`，標籤 `font-size:10px;opacity:.6`，值 `font-weight:700;font-size:13px`。
