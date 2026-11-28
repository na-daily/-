# 視覺風格：介紹頁與狀態面板
> 這套工作流的做法，非官方，整理自實際發布過的介紹頁與面板。MCP 指南列的是保守的安全屬性；這裡用到的 `transform`、`box-shadow`、`text-shadow`、`position`、`clip-path`、`object-fit`、各種漸層，在已發布的介紹頁和面板裡都正常顯示。

## 先定方向
1. **從作品的基調挑兩三個風格給使用者選**，每個用一句話說它長什麼樣：整頁做成什麼東西、什麼配色、哪一個招牌裝飾。使用者已經指定風格、或給了參考圖與別人的原始碼時，照他的做。
2. **介紹頁和狀態面板用同一套**：同樣的主色、同樣的底紋、同一個招牌裝飾，玩家從介紹頁進到聊天室時，一眼認得出是同一部作品。
3. **字體可以指定**，寫成字體堆疊，最後接一個通用字族（`sans-serif`、`serif`、`monospace`），裝置沒有那個字就往下退。數據、編號、系統訊息用 `monospace`，莊重的標題用 `serif`。

## 版面的做法
1. **整頁做成世界裡的一件東西**。先想玩家在這個世界最先拿到的是什麼：招募申請、觀光傳單、居民手冊、合約、檔案夾、平台後台。頁首用它的名義開場，各區塊是它的章節、欄位或附件，世界觀就藏在這件東西的口吻裡。
2. **笑點與不對勁放在版面上**：底下的小字附註、劃掉的字、塗黑的紀錄、只有一個答案的選項、星等評分。玩家自己讀得出來，內文照樣直白。
3. **頁面順序跟著世界的空間走**，底色跟著換：從地上往地下、從外圍往中心、從白天到夜裡。地圖用巢狀的框或一層一層的色帶畫，不用真的畫地圖。
4. **角色做成同一格式的卡**：圖、名字、身分、兩個標籤、一句他會說的台詞，每張篇幅一樣。用狀態燈或印章標出他屬於哪一邊，底細不寫。
5. **玩家模板做成世界裡的表單**：登記表、確認書、申請欄，勾選框、底線和印章都做出來。填寫格式和範例並排；身分有幾種就各做一份，收進 `details`。
6. **細節多的規則收進 `details`**，`summary` 寫玩家一看就懂的標題，打開才是內容。
7. **結尾做成一顆按鈕樣式的邀請**，文字用世界裡的動作（開始直播、簽名、入住），接在整頁最後一句鉤子後面。
8. **區塊用編號小標**（01、02…）或世界裡的欄位名當標題，同一頁的小標長得一樣。

## 限制裡的做法
- **畫面是靜態的**：沒有 `<style>`，所以沒有 `@keyframes`、`:hover` 和 `::before` 這類偽元素。閃爍的游標寫成一個「▌」字元，霓虹寫成固定的光暈；裝飾一律用實際的 `span`、`div` 做出來。
- **底紋用漸層疊**：面板的 style 不能用 `url()`，紙紋、格線、掃描線、木紋、放射光都用漸層疊。要照片質感的底時，把圖用 `position:absolute` 鋪在區塊底下、調低 `opacity`，上面的字加 `text-shadow`。
- **圖案轉成圖片放**：介紹頁和面板都放不了 `<svg>`。精緻的花紋邊框、徽章先做成 SVG，再轉成透明背景的 PNG 傳進圖庫，用 `img` 放；介紹頁和面板的 `img` 都吃圖庫網址。簡單的圖形用漸層、CSS 三角形與 Unicode 符號（❦ ✦ ⚜ ❀ ✧ ☾ ★☆）做。
- **互動只有展開與滑動**：介紹頁能點的只有 `details`；面板另外可以做左右滑動的分頁。

## 風格一覽
內建類別只用在狀態面板，依名稱對應，實際的樣子以預覽為準；沒有對應的就從自訂面板開始疊。

**紙與手作**
| 風格 | 適合的基調 | 整頁做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 可愛拼貼 | 陽光、日常、戀愛喜劇 | 筆記、合約、便條 | 圓點底、膠帶、便利貼、螢光筆底線、顏文字、大圓角 | `vv-ribbon` |
| 手帳日記 | 日常、校園回憶、慢熱 | 日記、交換日記 | 橫線紙、日期戳、拍立得、手寫感小標 | `vv-academy` |
| 鄉村告示 | 種田、慢活、小鎮 | 觀光傳單、告示板 | 木紋板、圖釘、歪斜的紙條、爆炸貼紙、小字附註 | `vv-pixel` |
| 卷宗檔案 | 調查、組織、反烏托邦 | 檔案夾、手冊 | 牛皮紙色、印章、等寬字的編號欄位、塗黑的字 | — |
| 報紙號外 | 懸疑、年代、事件開局 | 報紙頭版 | 粗大標題、雙線分隔、分欄、泛黃的底 | — |
| 信箋羊皮紙 | 奇幻、宮廷、宗教 | 書信、詔令 | 米黃漸層底、雙線金邊、置中標題、圓形蠟封 | `vv-rpg` |
| 和紙古風 | 和風、中式古風、大正浪漫 | 卷軸、帖子 | 素色底、朱紅印章、直排標題、細線框 | — |

**螢幕與介面**
| 風格 | 適合的基調 | 整頁做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 科技 HUD | 科幻、機甲、特務 | 任務簡報 | 深底、細線、四角角框、等寬字的數據列、單一亮色 | `vv-scifi` |
| 終端機 | 駭客、AI、末日的系統 | 系統日誌 | 黑底綠字或琥珀字、等寬字、`>` 提示符、掃描線 | — |
| 手機介面 | 現代都市、網路、直播、偶像 | 聊天室、App、平台後台 | 狀態列、聊天泡泡、通知卡、彈幕 | `vv-minimal` |
| 復古視窗 | 網路懷舊、怪談、Y2K | 彈出視窗 | 灰底立體邊框、藍色標題列、✕ 按鈕 | — |
| 遊戲選單 | RPG、冒險、異世界 | 角色建立畫面 | 深藍底白框、HP／MP 量表、裝備格、▶ 游標 | `vv-rpg` |
| 戀愛遊戲 | 戀愛、攻略、多人選擇 | 攻略對象選擇畫面 | 半透明對話框、名牌、好感愛心 | `vv-dating` |

**光與暗**
| 風格 | 適合的基調 | 整頁做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 霓虹賽博 | 都市夜、賽博、地下世界 | 直播申請、夜店傳單 | 深底、霓虹雙色、發光邊框、斜切角、ON AIR 標籤 | `vv-neon` |
| 哥德暗黑 | 壓抑、背德、吸血鬼、惡魔 | 邀請函、契約 | 黑與酒紅、細金線、對稱的花飾符號、襯線標題 | `vv-gothic` |
| 夢核 Y2K | 夢境、怪誕可愛、虛擬世界 | 個人網頁 | 粉藍紫漸層、星星符號、果凍感的大圓角、低飽和 | `vv-crystal` |
| 明亮的反烏托邦 | 管控、偽善、宗教國家 | 宣傳手冊 | 鮮黃或純白的底、放射光、整齊的笑臉符號、紅色警示框 | — |

**質樸與文件**
| 風格 | 適合的基調 | 整頁做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 像素 | 農場、懷舊遊戲 | 遊戲標題畫面 | 方角、粗實線邊框、錯位的實心陰影、格子底 | `vv-pixel` |
| 極簡雜誌 | 成熟、都會、職場 | 雜誌專題 | 大留白、大字號標題、髮絲線、黑白加一個強調色 | `vv-minimal` |
| 病歷報告 | 末日、醫療、實驗、收容 | 收容紀錄 | 表格欄位、編號、紅筆批註、警示色條 | `vv-survival` |
| 卡牌 | 奇幻、命運、多角色 | 牌組 | 每個角色一張直式牌面、牌框、牌背花紋 | — |

## 構成元件的寫法
顏色換成作品的配色。面板裡的文字與底色照面板的顏色規則寫，下面寫死的顏色只用在裝飾。

**底紋**
- 圓點：`background:radial-gradient(circle at 12px 12px,#ffd9c7 3px,transparent 4px) 0 0/28px 28px,#ffeadd`。
- 橫線紙：`background:repeating-linear-gradient(transparent 0 27px,rgba(110,140,190,.35) 27px 28px);line-height:28px`。行高和線距一樣，字才會落在線上。
- 格子：`background:linear-gradient(rgba(0,0,0,.06) 1px,transparent 1px) 0 0/16px 16px,linear-gradient(90deg,rgba(0,0,0,.06) 1px,transparent 1px) 0 0/16px 16px`。
- 掃描線：`background:repeating-linear-gradient(0deg,rgba(0,0,0,.25) 0 1px,transparent 1px 3px),#0b0f0a`。
- 木紋：`background:repeating-linear-gradient(90deg,#b98a55 0 10px,#b08250 10px 12px)`，外面再包一圈深色粗邊當板框。
- 斜紋：`background:repeating-linear-gradient(135deg,#ffe4d6 0 10px,#ffd8c6 10px 20px)`，適合面板的頂欄。
- 放射光：`background-image:repeating-conic-gradient(from 0deg at 50% 108%,rgba(255,255,255,.28) 0 5deg,transparent 5deg 10deg)`，配一個同中心的 `radial-gradient` 亮斑。
- 牛皮紙：`background:linear-gradient(135deg,#e8d3a9,#d9bf8c 60%,#cfb07a)`；羊皮紙在前面再疊一層 `radial-gradient(ellipse at center,transparent 55%,rgba(90,60,20,.25))` 做暗角。
- 霓虹夜：兩團對角的 `radial-gradient(ellipse 90% 60% at 100% 0,rgba(255,43,214,.32),transparent)` 加青色的另一團，疊在深紫底上。

**邊框**
- 雙線框（信箋、報紙、印章）：`border:3px double #8a6d3b`。
- 像素框：`border:3px solid #3b2a1a;border-radius:0;box-shadow:4px 4px 0 #3b2a1a`。
- 厚底卡片（可愛風）：`border-radius:22px;box-shadow:0 5px 0 #ffcdb8`，不加模糊的實心陰影。
- 復古視窗：外框 `background:#c0c0c0;border:2px solid;border-color:#fff #808080 #808080 #fff`，標題列 `background:#000080;color:#fff;padding:2px 6px;display:flex;justify-content:space-between`，右邊放一個「✕」。
- 霓虹：深底加 `border:1px solid #ff3df0;color:#ff3df0;box-shadow:0 0 6px #ff3df0,inset 0 0 6px #ff3df0;text-shadow:0 0 6px #ff3df0`。
- 單邊亮條（HUD、檔案卡）：`border-left:3px solid 亮色` 加 `border-radius:4px 14px 4px 14px`。
- HUD 角框：外層 `position:relative`，四個角各放一個 `position:absolute;width:10px;height:10px` 的 span，左上角寫 `top:0;left:0;border-top:2px solid;border-left:2px solid`，其他三角照方向換。
- 斜切角：`clip-path:polygon(0 0,calc(100% - 12px) 0,100% 12px,100% 100%,12px 100%,0 calc(100% - 12px))`；屋頂、牌匾用 `clip-path:polygon(50% 0,100% 18px,100% 100%,0 100%,0 18px)`。

**裝飾件**
- 印章：`display:inline-block;border:2px solid #c0392b;color:#c0392b;padding:2px 10px;border-radius:4px;font-weight:bold;letter-spacing:.2em;transform:rotate(-8deg);opacity:.85`；圓章改成固定寬高加 `border-radius:50%`，字用 `<br>` 斷成兩三行。
- 膠帶：卡片最上面放一個 `display:block;width:70px;height:18px;margin:-20px auto 4px;background:rgba(255,220,120,.6);transform:rotate(-3deg)` 的 span，負的上邊距讓它壓在卡片邊緣，數字照卡片的上內距加 9px 調。兩條膠帶時用 `display:flex;justify-content:space-between` 排在卡片上方，再給卡片負的上邊距。
- 圖釘：紙條 `position:relative`，裡面放 `position:absolute;top:-5px;left:50%;width:10px;height:10px;margin-left:-5px;border-radius:50%;background:#c8322b`。
- 便利貼：`padding:8px 12px;background:#fff1a8;transform:rotate(-1.5deg);box-shadow:2px 3px 0 rgba(0,0,0,.12);border-radius:2px 2px 12px 2px`，接在條文下面當反駁或批註，每張換一個顏色、一個角度。
- 拍立得：`background:#fff;padding:5px 5px 18px;box-shadow:0 2px 5px rgba(0,0,0,.25);transform:rotate(-1.5deg)`，圖用 `object-fit:cover` 裁成固定高度，下緣寫一行小字說明。
- 爆炸貼紙：`position:absolute` 的圓，`border:3px dashed;transform:rotate(12deg)`，裡面用 `display:flex;align-items:center;justify-content:center` 置中兩三行字。
- 螢光筆：`background:linear-gradient(transparent 60%,#ffe27a 60%)`；波浪底線：`text-decoration:underline wavy #ff8a70;text-underline-offset:8px`。
- 塗黑：`<span style="background:#1d2238;color:#1d2238">████████</span>`，只是裝飾，真的要藏的內容本來就不放進介紹頁。
- 劃掉重寫：`<s style="opacity:.6">原本的字</s>` 後面接改過的字。
- 狀態燈：`display:inline-block;width:6px;height:6px;border-radius:50%;background:#6fa886`，綠、黃、紅、灰各代表一種身分或狀態。
- CSS 三角形（貓耳、箭頭）：`width:0;height:0;border-left:20px solid transparent;border-right:20px solid transparent;border-bottom:24px solid #fff`。
- 聊天泡泡：`display:inline-block;max-width:75%;padding:8px 12px;border-radius:16px 16px 16px 4px`；自己這邊的泡泡包一層 `text-align:right`，圓角改成 `16px 16px 4px 16px`。
- 按鈕：`display:inline-block;padding:10px 30px;border-radius:999px;font-weight:800;letter-spacing:.2em`，可愛風加 `box-shadow:0 5px 0 深色`，霓虹風加外光暈。
- 直排標題（古風）：`writing-mode:vertical-rl`，只用在短標題。
- 量表跟著風格換樣子：像素用 `{{gauge}}` 的 ■□，HUD 用 4px 高的細條，可愛風用圓角粗條加漸層，哥德和戀愛用 `{{repeat}}` 排愛心或玫瑰。

## 自查
- 整頁看得出是世界裡的哪一件東西嗎？
- 介紹頁和面板放在一起看，認得出是同一部作品嗎？
- 每個區塊是不是只有一個招牌裝飾？
- 深色和淺色模式都看過一次嗎？
