# 狀態面板 variableViewHtml
> 高度、`<style>` 無效、字數上限是官方規則；標籤、屬性、CSS 變數與模板語法來自早期官方說明，工具報錯時以報錯為準；兩種版型與內層捲動是實測做法。

## 規格
**能用的東西**
- 上限 10,240 字。只能用 inline style 和官方 CSS 變數，可以用 emoji；`img` 放圖庫網址可以顯示（實測），適合做角色的小頭像。
- 不能用 `<script>`、外部 CSS／JS；style 裡不能用 `expression()`、`url()`、`@import`。
- 允許的標籤：h1～h6、p、br、hr、strong、em、b、i、u、s、del、mark、small、sub、sup、ul、ol、li、dl、dt、dd、a、img、figure、figcaption、blockquote、pre、code、table 系列、div、span、section、article、details、summary、video、source（`video` 標籤在，但外部影片網址不會播，實測）。
- 允許的屬性：href、target、rel、src、alt、width、height、class、id、style、colspan、rowspan、scope、controls、autoplay、loop、muted、playsinline、open、loading。
- 沒有 input、label、button，`details` 也沒有 `name`，所以按鈕式的面板只能用各自獨立的 `details` 展開，做不出點一個、關掉其他的互斥效果。

**顏色**
- 要跟著深色、淺色模式變時，文字、背景、邊框用 CSS 變數：--background、--foreground、--surface、--surface-elevated、--text-secondary、--text-label、--border-subtle、--border、--color-primary、--color-negative 等，寫死的顏色只用在裝飾。半透明用 `color-mix(in srgb, var(--foreground) N%, transparent)`。
- 要固定的深色面板（終端機、霓虹、哥德）時，底色和文字色一起寫死，兩種模式看起來就都一樣；外層再加 `color-scheme:dark;scrollbar-width:thin`，捲軸才不會是亮白色。

**模板語法**
- 取值：`{{變數名}}`、`{{變數名:numeric}}`（取出 0～100 的數字，用在 `width:N%`）、`{{user}}`、`{{charN}}`、`{{#each variables}}…{{/each}}`（內有 name、value、numericValue）。
- 條件：`{{#range 變數 min max}}`（min ≤ 值 < max）、`gt`、`gte`、`lt`、`lte`、`eq`、`neq`，比較的全是數字。
- 數學：`{{math 變數 + 值}}`，由左到右計算，可以接 `| round`、`| floor`、`| ceil`。
- 格式：`{{repeat 字 變數 單位}}`、`{{gauge 變數 填滿 空格 格數}}`、`{{icon 變數 門檻:圖示 …}}`。
- 含「｜」的複合文字變數會整串顯示，條件與數學只吃數字；要拆成量表時改成獨立的數字變數。多行的文字變數，那一格加 `white-space:pre-line`。

## 高度 300px
面板超過 300px 下方會被裁切，也不會出現捲軸（官方限制）。同一個面板的實際高度會隨手機的寬度和系統字體大小改變：窄的手機、放大的字體會讓一行字折成兩三行，寬的手機一行可以放很長，所以高度沒辦法照行數算準，要照最窄、字最大的情況留餘裕。
- 卡片橫向排列（`flex-wrap`），常駐的資訊只放一張緊湊的卡片，其餘收進展開。
- 會變長的內容（文字變數、名單、約定）放進有 `max-height` 與 `overflow-y:auto` 的容器，讓它在裡面自己捲，長度怎麼變都不會撐破面板。
- 只該佔一行的欄位（名字、地點、標籤）加 `white-space:nowrap;overflow:hidden;text-overflow:ellipsis`，太長時以「…」收尾，不會折行把下面擠出去。
- 寬度用百分比或 `flex`，字級用 `em`，不寫死 px 寬度。整體字級可以寫 `font-size:clamp(12.5px,3.6vw,14px)`，跟著手機寬度縮放。

## 兩種實測過的版型
**左右滑動的分頁**：適合每個角色、每個區塊各佔一頁的面板。
- 最外層 `height:300px;display:flex;flex-direction:column;box-sizing:border-box;overflow:hidden`。
- 最上面一列固定不動（`flex:none`），放時間、地點這類每則都會變的場景變數，用固定高度加 `white-space:pre;overflow:hidden` 限在兩行內。
- 中間是滑動區：`flex:1;min-height:0;display:flex;overflow-x:auto;overflow-y:hidden;scroll-snap-type:x mandatory;scrollbar-width:none`。每一頁 `flex:0 0 100%;box-sizing:border-box;scroll-snap-align:start;overflow-y:auto;scrollbar-width:none`，內容在頁裡自己往下捲。
- 一頁一個主題：{{user}} 自己、每個角色各一頁、名冊、指令表。捲軸藏起來了，所以每頁的標題列標上頁碼（◆◇◇◇ 或 02 / 06），玩家才知道還能滑；介紹頁或創作者留言也寫一句「狀態面板可以左右滑」。
- 最下面可以再固定一行作品標語或提示。

**流內展開的圖示格子**：適合項目多、每項內容短的面板。點圖示時內容在圖示列下方展開，再點一次收起，展開的內容在流內，不是浮層。
- 最外層 `max-height:280px;overflow-y:auto`，由內層自己捲動，展開再多也不會被外層裁掉。
- 每一排是 `position:relative;padding-top:<圖示列高度>px` 的 div；每個項目一個 `<details style='display:block'>`，它的 `<summary>` 用 `position:absolute;top:0;left:N%;width:20%`（五格就是 0、20、40、60、80%），圖示疊在同一排，不佔流內的高度。
- 展開內容的 div 放在 `details` 裡、在流內（固定高度加 `overflow-y:auto`），展開時把下面的東西往下推。可以同時展開好幾個，由外層捲動承接；全部收合時約 242px。
- 圖示加徽章加名字這類從變數截取出來的小元件，一樣放在 summary 裡。
- 沒有固定的中間顯示區，所以留一行小字提示「點選圖示展開，再點一次收起」。

## 值沒顯示時
- 面板原樣出現 `{{名稱}}`：沒有這個名稱的變數，檢查拼字。
- 顯示「—」：值是空的，檢查作品設定的更新總則。
- 下半被裁切：高度超過 300px。

## 自查
- 請使用者在自己的手機預覽過了嗎？最好也把系統字體調大看一次。
- 會變長的內容都放進內層捲動的容器了嗎？
- 變數的名稱或行數改過之後，面板回頭對過了嗎？
