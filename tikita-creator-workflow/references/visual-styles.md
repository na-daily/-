# 視覺風格：介紹頁與狀態面板
> 這套工作流的做法，非官方，整理自實際發布過的介紹頁與面板。MCP 指南列的是保守的安全屬性；這裡用到的 `transform`、`box-shadow`、`text-shadow`、`position`、`clip-path`、`object-fit`、各種漸層，在已發布的介紹頁和面板裡都正常顯示。

## 怎麼用
使用者沒指定風格時，從下面挑幾個符合作品基調的風格，每個用一句話說它長什麼樣，讓他選；他指定了風格、或給了參考圖與別人的原始碼時，照他的做。

## 能做到的事
沒有 `<style>` 也沒有 `<script>`，畫面全靠 inline style、`details`、捲動與面板的模板語法。標「未實測」的照語法可行，第一次用時預覽確認；其餘都在 Tikita 上試過。

**像在互動**
- 展開收合（`details`）：規則與須知折起來；劇透開關，點了才看到；抽牌，`summary` 做牌堆、展開在下面出現抽到的牌（`summary` 展開後還在原處，所以做不出牌背翻成牌面）；選擇題，每個選項一個 `details`，打開看結果；`details` 套 `details` 做分支。加 `open` 屬性就預設展開。
- 左右滑動：`overflow-x:auto` 加 `scroll-snap-type:x mandatory`，做分頁、角色卡輪播、相簿。介紹頁和面板都可以。
- 內層捲動：長的清單、日誌、對話紀錄放進固定高度加 `overflow-y:auto` 的框，在框裡自己捲。
- 頁內跳轉：`<a href="#區塊id">` 配上區塊的 `id`，做目錄或「回到頂端」。

**會變的畫面（只有面板）**
- 依變數換字：`{{#range}}` 和比較條件包住不同的文字，好感、時段、狀態一變，顯示的稱號或心境就跟著換。
- 依變數換樣子：條件包住不同的 `img`、不同顏色的 `span`，或把 `{{變數名:numeric}}` 填進 `width`、`opacity`，做出換表情的頭像、隨時段換的底色、會長會縮的量表。
- 依數字排圖示：`{{repeat}}`、`{{gauge}}`、`{{icon}}` 排出愛心、方塊、星等。

**會動的東西**
- CSS 動畫做不到，要動的話用會動的圖：動態的 WebP 或 GIF 傳進圖庫，用 `img` 放，介紹頁和面板都會動。
- 面板允許 `video`，配 `autoplay loop muted playsinline` 做無聲循環的小動畫（未實測）。
- 閃爍的游標、霓虹燈這類效果寫成靜態的樣子：游標是一個「▌」字元，霓虹是固定的光暈。

**畫面的樣子**
- 漸層疊出底紋：面板的 style 不能用 `url()`，紙紋、格線、掃描線、木紋、放射光都用 `linear-gradient`、`radial-gradient`、`repeating-conic-gradient` 疊。
- 陰影、旋轉、裁切形狀：`box-shadow`、`text-shadow`、`transform:rotate()`、`clip-path`、`border-radius`。
- 疊圖：圖用 `position:absolute` 鋪在區塊底下、調低 `opacity`，上面的字加 `text-shadow`；圖用 `object-fit:cover` 和 `object-position` 裁成需要的比例。
- 濾鏡：`filter:grayscale()`、`sepia()`、`blur()` 做褪色的舊照片或模糊的背景。
- 圖案：介紹頁和面板都放不了 `<svg>`。精緻的花紋邊框、徽章先做成 SVG，再轉成透明背景的 PNG 傳進圖庫，用 `img` 放；簡單的圖形用漸層、CSS 三角形與 Unicode 符號（❦ ✦ ⚜ ❀ ✧ ☾ ★☆）做。
- 裝飾一律用實際的 `span`、`div`，因為 `::before` 這類偽元素要寫在 `<style>` 裡。
- 字體：寫成字體堆疊，最後接一個通用字族（`sans-serif`、`serif`、`monospace`），裝置沒有那個字就往下退。
- 直排：`writing-mode:vertical-rl`，適合短標題。
- 表格：`table` 系列可以用，做價目表、時刻表、成績單。

## 風格一覽
內建類別只用在狀態面板，依名稱對應，實際的樣子以預覽為準；沒有對應的就從自訂面板開始疊。

**紙與手作**
| 風格 | 適合的基調 | 可以做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 可愛拼貼 | 陽光、日常、戀愛喜劇 | 筆記、合約、便條 | 圓點底、膠帶、便利貼、螢光筆底線、顏文字、大圓角 | `vv-ribbon` |
| 手帳日記 | 日常、校園回憶、慢熱 | 日記、交換日記 | 橫線紙、日期戳、拍立得、手寫感小標 | `vv-academy` |
| 鄉村告示 | 種田、慢活、小鎮 | 觀光傳單、告示板 | 木紋板、圖釘、歪斜的紙條、爆炸貼紙、小字附註 | `vv-pixel` |
| 卷宗檔案 | 調查、組織、反烏托邦 | 檔案夾、手冊 | 牛皮紙色、印章、等寬字的編號欄位、塗黑的字 | — |
| 報紙號外 | 懸疑、年代、事件開局 | 報紙頭版 | 粗大標題、雙線分隔、分欄、泛黃的底 | — |
| 信箋羊皮紙 | 奇幻、宮廷、宗教 | 書信、詔令 | 米黃漸層底、雙線金邊、置中標題、圓形蠟封 | `vv-rpg` |
| 和紙古風 | 和風、中式古風、大正浪漫 | 卷軸、帖子 | 素色底、朱紅印章、直排標題、細線框 | — |

**螢幕與介面**
| 風格 | 適合的基調 | 可以做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 科技 HUD | 科幻、機甲、特務 | 任務簡報 | 深底、細線、四角角框、等寬字的數據列、單一亮色 | `vv-scifi` |
| 終端機 | 駭客、AI、末日的系統 | 系統日誌 | 黑底綠字或琥珀字、等寬字、`>` 提示符、掃描線 | — |
| 手機介面 | 現代都市、網路、直播、偶像 | 聊天室、App、平台後台 | 狀態列、聊天泡泡、通知卡、彈幕 | `vv-minimal` |
| 復古視窗 | 網路懷舊、怪談、Y2K | 彈出視窗 | 灰底立體邊框、藍色標題列、✕ 按鈕 | — |
| 遊戲選單 | RPG、冒險、異世界 | 角色建立畫面 | 深藍底白框、HP／MP 量表、裝備格、▶ 游標 | `vv-rpg` |
| 戀愛遊戲 | 戀愛、攻略、多人選擇 | 攻略對象選擇畫面 | 半透明對話框、名牌、好感愛心 | `vv-dating` |

**光與暗**
| 風格 | 適合的基調 | 可以做成 | 構成 | 面板可先套 |
|---|---|---|---|---|
| 霓虹賽博 | 都市夜、賽博、地下世界 | 直播申請、夜店傳單 | 深底、霓虹雙色、發光邊框、斜切角、ON AIR 標籤 | `vv-neon` |
| 哥德暗黑 | 壓抑、背德、吸血鬼、惡魔 | 邀請函、契約 | 黑與酒紅、細金線、對稱的花飾符號、襯線標題 | `vv-gothic` |
| 夢核 Y2K | 夢境、怪誕可愛、虛擬世界 | 個人網頁 | 粉藍紫漸層、星星符號、果凍感的大圓角、低飽和 | `vv-crystal` |
| 明亮的反烏托邦 | 管控、偽善、宗教國家 | 宣傳手冊 | 鮮黃或純白的底、放射光、整齊的笑臉符號、紅色警示框 | — |

**質樸與文件**
| 風格 | 適合的基調 | 可以做成 | 構成 | 面板可先套 |
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
- 量表跟著風格換樣子：像素用 `{{gauge}}` 的 ■□，HUD 用 4px 高的細條，可愛風用圓角粗條加漸層，哥德和戀愛用 `{{repeat}}` 排愛心或玫瑰。

## 自查
- 深色和淺色模式都看過一次嗎？
