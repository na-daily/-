# 狀態面板 variableViewHtml
> 高度、`<style>` 無效、字數上限是官方規則；標籤、屬性、CSS 變數與模板語法來自早期官方說明，工具報錯時以報錯為準；圖示格子與內層捲動是實測做法。

## 規格
- 上限 10,240 字。只能 inline style 與官方 CSS 變數；不能 `<script>`、外部 CSS／JS，style 內不能用 `expression()`、`url()`、`@import`。
- 允許標籤：h1～h6、p、br、hr、strong、em、b、i、u、s、del、mark、small、sub、sup、ul、ol、li、dl、dt、dd、a、img、figure、figcaption、blockquote、pre、code、table 系列、div、span、section、article、details、summary、video、source。
- 允許屬性：href、target、rel、src、alt、width、height、class、id、style、colspan、rowspan、scope、controls、autoplay、loop、muted、playsinline、open、loading。沒有 input、label、button，`details` 也沒有 `name`，所以按鈕式面板只能用各自獨立的 `details` 展開，做不出互斥。
- 文字、背景、邊框用 CSS 變數（--background、--foreground、--surface、--surface-elevated、--text-secondary、--text-label、--border-subtle、--border、--color-primary、--color-negative 等），才能同時支援深色與淺色模式；寫死的顏色只用在裝飾。半透明用 `color-mix(in srgb, var(--foreground) N%, transparent)`。
- 模板語法：`{{變數名}}`、`{{變數名:numeric}}`（取出 0～100 的數字，用在 `width:N%`）、`{{user}}`、`{{charN}}`、`{{#each variables}}…{{/each}}`（內有 name、value、numericValue）。
- 條件：`{{#range 變數 min max}}`（min ≤ 值 < max）、`gt`、`gte`、`lt`、`lte`、`eq`、`neq`，比較全是數字。數學：`{{math 變數 + 值}}`，由左到右計算，可接 `| round`、`| floor`、`| ceil`。格式：`{{repeat 字 變數 單位}}`、`{{gauge 變數 填滿 空格 格數}}`、`{{icon 變數 門檻:圖示 …}}`。
- 內建 CSS 類別：vv-rpg、vv-neon、vv-crystal、vv-dating、vv-pixel、vv-survival、vv-ribbon、vv-minimal、vv-gothic、vv-academy、vv-scifi。可以用 emoji。
- 含「｜」的複合文字變數整串顯示，條件與數學只吃數字；要拆成量表就改成獨立的數字變數。多行文字變數的那一格加 `white-space:pre-line`。

## 高度 300px
面板超過 300px 下方會被裁切，也不會出現捲軸（官方限制）。同一個面板的實際高度會隨手機的寬度和系統字體大小改變：窄的手機、放大字體時，一行字會折成兩三行；寬的手機一行可以放很長。所以高度沒辦法照行數算準，要照最窄、字最大的情況留餘裕：
- 卡片橫向排列（`flex-wrap`），常駐資訊只放一個緊湊的卡片，其餘收進按鈕。
- 會變長的內容（文字變數、名單、約定）放進有 `max-height` 與 `overflow-y:auto` 的容器，讓它在裡面自己捲，長度再怎麼變都不會撐破面板。
- 只該佔一行的欄位（名字、地點、標籤）加 `white-space:nowrap;overflow:hidden;text-overflow:ellipsis`，太長就以「…」收尾，不會折行把下面擠出去。
- 寬度用百分比或 `flex`，字級用 `em`，不寫死 px 寬度。
- 寫完請使用者在自己的手機預覽，最好也把系統字體調大看一次；變數的行數改了就回頭檢查。

**首選：流內展開的圖示格子**（實測）。展開內容在圖示列下方展開、再點收起，不是浮層：
- 最外層 `max-height:280px;overflow-y:auto`，內層自己捲動，展開再多也不會被外層裁掉。深色面板在外層加 `color-scheme:dark;scrollbar-width:thin`，否則捲軸是亮白色。
- 每一排是 `position:relative;padding-top:<圖示列高度>px` 的 div；每個項目一個 `<details style='display:block'>`，它的 `<summary>` 用 `position:absolute;top:0;left:N%;width:20%`（五格就是 0、20、40、60、80%），圖示疊在同一排，不佔流內高度。
- 展開內容的 div 放在 `details` 裡、在流內（固定高度加 `overflow-y:auto`），展開時把下面的東西往下推。可以同時展開多個，由外層捲動承接；收合時約 242px。
- 圖示加徽章加名字這類由變數截取出的小元件，一樣放在 summary 裡。沒有固定的中間顯示區，留一行小字提示「點選圖示展開，再點一次收起」。

使用者提供別人的面板原始碼時，先讀它的結構再套用。

## 值沒顯示時
- 面板原樣出現 `{{名稱}}`：沒有這個名稱的變數，檢查拼字。
- 顯示「—」：值是空的，檢查作品設定的更新總則。
- 下半被裁切：高度超過 300px。
