# 編輯草稿的工具與坑、圖庫
> 官方工具說明與實測。工具說明或報錯與這裡不符時，以工具為準並更新這份檔案。
- 呼叫方式依環境而定：一般 MCP 環境直接呼叫 `tikita` 工具；Code Mode 環境（如 OpenCode）在 `execute` 裡以 `await tools.tikita.<tool_name>({...})` 呼叫。

## 找草稿與讀取
- 找草稿：`story_list_my_stories`（`query`、`offset`、`has_more`），或把 `/create/<short_id>`、`/story/<short_id>` 網址直接放進任何工具的 `short_id` 欄。
- 讀：`workspace_list_items` 列出項目，`workspace_get_item` 讀單一項目；需要整份才用 `workspace_get_draft`。
- 使用者在 Studio 改過的內容以 Studio 為準，寫入前先讀最新版。Studio 存檔後 item id 會變，要重新 list。

## 選哪個工具
| 要改的 | 用 | 注意 |
|---|---|---|
| 長欄位裡的一小段 | `workspace_replace_text` | `find` 必須唯一，`find` 與 `replace` 各 4,000 字內，不支援正規表達式；零個或多個符合都會拒絕，加長前後文再試。`text_not_found` 通常代表使用者改過，重讀再試。可用於 story、openings、characters、episodes、variables、dialogs |
| 單一開場、角色、章節、變數、範例對話的幾個欄位 | `workspace_patch_item` | 帶既有 id，只送要改的欄位，其他欄位與其他項目不動；可帶 `expected_updated_at`，遇到衝突先重讀。不能新增項目 |
| 單一故事欄位（標語、標籤、detailMd、introHtml…） | `workspace_update_story_metadata` | 加 `return_mode: "summary"`。這個工具的 `status` 欄位不要用，公開與否由創作者在 Studio 操作。`openings` 在這裡是整個清單：省略的已發布開場會在 Studio 存檔時被刪除，保留每個 `id` 與 `originId` |
| 新增角色、章節、設定集連結，或大量改動 | `workspace_patch_draft` | 陣列整個取代，見下一節。加 `return_mode: "summary"` |
| 設定集條目內容 | `lorebook_patch_entry`（只送改的欄位）；整條新建或取代用 `lorebook_upsert_entry` | 設定集庫改動要使用者在 Studio 存檔後才進對話，存檔前草稿裡套用的副本可能還是舊的。`lorebook_delete_entry` 會連同所有作品的套用一起刪除，只在使用者明說時用；只想從這部作品拿掉用 `lorebook_detach_from_workspace_draft` |

## `workspace_patch_draft` 的陣列
- 角色、章節、設定集：省略既有 id 會被拒絕，除非 `confirm_array_drop: true`（只在使用者明說刪除時用）。
- 角色：比對到的角色省略 name、character_intro、secret（或給 null）會保留原值，給空字串才清空。gender、age、display_order、avatar_image_id 沒有這個保證（實測只帶 id 加 secret 時年齡、性別、順序都會消失），每個角色都照樣帶齊。只改一段文字時優先用 `workspace_replace_text` 或 `workspace_patch_item`；`find` 要注意全形標點（「：」不是「:」）。
- 第 1 張角色卡無法刪除；調整卡片順序會改變 `{{char1}}`、`{{char2}}` 的編號，調序後檢查開場與範例對話。
- 章節（實測）：每個舊章節都帶齊 `id`、`originId`、`display_order`、`title`、`body_md`、`secret`、`transition_condition`，Studio 存檔後多出的 `start_bg_image_id`、`start_bg_image_url` 也一起帶上。漏送 `originId` 會斷掉和正式版章節的連結。送完立刻拉回比對。
- `story.openings` 同樣整個取代（最多 5 個），保留每個 id、originId、title、body、chatStarters。
- 其他陣列（範例對話、變數、圖片）沒有 id 防呆，省略就會消失。

## 開場的鏡像欄位
- 故事層的 `firstMessage` 與 `chatStarters` 是第一個開場的相容鏡像。送出非空的 `openings` 清單時，MCP 會自動產生這兩個鏡像；只改這兩個舊欄位時，只會更新第一個開場，其他開場保留。
- 只有一個開場的舊式草稿：不送 `openings`，改送 `firstMessage` 加剛好 3 個 `chatStarters`。送空清單或不送清單都走舊路徑，空清單不會刪掉已發布的開場。
- `story_get_creation_context` 會回傳已部署的開場（依 `position,id` 排序，含不可變的 `stable_position` 與啟用狀態），那裡的欄位叫 `chat_starters`。編輯時一律用工作區的形狀（`chatStarters`、`originId`），把已發布的 UUID 放在 `originId`，不要把 `position` 或 `stable_position` 寫進工作區。

## 範例對話
上限 5 組。要加新的就要換掉舊的，先告知使用者換掉哪一組。

## 留言與貼文
- 留言：`comment_list_recent` 看自己作品與貼文的最新留言，`comment_list_thread` 看某則公開留言串，`comment_create` 以使用者帳號發留言或回覆。發出去就是公開的，只在使用者明確要求、並確認內容後才發。
- 貼文：`post_list_my_drafts`、`post_create_draft`、`post_update_draft` 只處理伺服器上的貼文草稿，不會建立或修改公開貼文，發布在 Tikita 上由使用者操作。

## 其他
- 中文直接寫，不要把整段轉成 `\ u` 跳脫碼；跳脫過的字很容易打錯成形近字。寫完一定讀回對照。
- 大量寫入前，或遇到 `story_total_length_exceeded`，先看 `workspace_get_content_budget`；該錯誤表示草稿沒存成，回報上限並減內容後再試，不要說成是宿主擋下的請求。
- 頻率上限：讀取每分鐘 100 次、寫入每分鐘 60 次（`get_capabilities` 的 `rate_limit`）。
- `whoami`、`diagnostics_audit_log` 只用在帳號確認與除錯。

# 圖庫
MCP 的上傳與附加工具在本檔最後。

## 圖片資產與圖片群組
- 從「我的圖片庫」挑選的圖片會成為圖片資產，放進圖片群組，或保留為未分類。一次可上傳最多 10 張。發布至少要 1 張圖庫圖片加說明。
- 群組名稱 50 字，圖片說明 100 字（Cinema 場景圖說明 30 字），都計入 30,000 字，圖片有數百張時說明會累積，要簡短具體。
- **AI 會看群組名稱與圖片說明來選圖**，檔名只是輔助。群組名稱就是 AI 的提示，依地點（辦公室／家／咖啡廳）、時間（白天／夜晚）、氛圍（爭吵／和解／緊張）取名，讓使用時機一目瞭然。
- 一張圖片只能放在一個群組；刪除群組，圖片會變成未分類。**未分類圖片超過 50 張就無法儲存**，想不到名稱時建「其他」這類寬泛群組。
- 檔名與說明：地點・時間・人物・狀態順序，例如「辦公室_加班_夜晚.jpg」「徐俊_嚴肅表情_特寫.jpg」；說明可寫成句子，「下雨的夜晚，熄燈的辦公室」效果最好。中文可以。

## 縮圖與代表圖片
- 靜態縮圖 1 張（發布必填）；動態縮圖 1 張（選填，在圖庫步驟用已上傳圖片建立）；Cinema 縮圖最多 6 張，第一張為代表圖。每張上限 4MB，超過上傳時會自動縮小。
- 縮圖屬公開區域（最嚴標準），成人作品也不能加入直接描寫。

## 秘密圖片與解鎖條件
- 在對話中解鎖的圖片。設定位置：圖庫點選圖片開啟「編輯圖片」視窗，把秘密開啟後，解鎖費用與最低解鎖條件欄位才會展開；多張用「批次編輯」。**只按卡片上的公開／秘密切換鈕不會出現條件欄位，會用預設值（50 Tik、無對話次數條件）設為秘密。**
- 解鎖費用 5～999 Tik（預設 50，不能留白），是使用者想提前在圖庫解鎖時的價格。最低解鎖條件：50 次以上、以 10 為單位，可關閉；開啟後在超過該對話次數前，自動解鎖與圖庫購買都不行，用於想留到後面的圖片；關閉的話第一次對話就可能出現。
- 解鎖途徑：對話中自動解鎖（AI 選圖時秘密圖也是候選，超過最低條件就免費解鎖）；在角色圖庫用 Tik 或圖片解鎖券直接解鎖。解鎖過一次對該使用者永久保留。
- 開場不能放秘密圖片或成人圖片。被自動判定攔下時，用圖片庫的筆刷遮住後儲存。

## 聊天圖片模式
- 背景模式：AI 在回覆結尾選好場景，切換聊天室背景；適合地點與氛圍重要、準備了很多背景圖的作品。
- 內嵌模式：AI 在對話中途像傳訊軟體一樣傳圖；適合會互傳照片的關係。Cinema 作品固定為內嵌模式。
- MCP 欄位 `chatImageMode`（`background` 或 `inline`）。

## 用 MCP 上傳與附加（官方 MCP 指南）
- 上傳：`asset_create_upload_slot` → 用簽名網址 PUT 位元組（不是 MCP 呼叫，網址不可顯示）→ `asset_finalize_upload` → `asset_attach_to_workspace_draft`。外部網址用 `asset_import_from_url` → attach。整理用 `asset_create_group`、`asset_move_to_group`（預設用工作區連結 id，只有圖庫 id 時帶 `match_by: "user_image_id"`）。
- 能讀到本機檔時，在 `asset_create_upload_slot` 帶上確切的寬、單格高度與 `frame_count`，不支援的動圖會在上傳前就被擋下。
- 一張上傳要 3 次寫入、網址匯入 2 次；預設寫入上限每分鐘 60 次。遇到 `rate_limit_exceeded` 就停下回報剩下的工作，不迴圈重試。
- `retryable: false` 的錯誤不要用同一來源重試，照回報的限制改檔案再建新的上傳槽。可重試的處理錯誤只用原本的 finalize 參數重試，不要建重複的上傳槽；`retry_reason` 是 `storage_cleanup_failed` 時，等一下再 finalize 最多一次。
- 上傳槽不是圖庫圖片，要等 `asset_finalize_upload` 回傳完成、NSFW 等級（`sfw`、`soft_nsfw`、`hard_nsfw`）與原圖和鎖定圖兩個網址才算，之前不要附加或引用它的 id。
- 完成後用 `workspace_get_draft` 與 `asset_list_groups` 核對。上傳後只回報張數、連結類型、說明與工作區，不貼簽名網址或原始回應。
- 對話裡會被 AI 選用的圖，群組照上面「圖片資產與圖片群組」的做法依地點、時間、氛圍取名。不在對話裡出現的圖（封面、頭像）另外放一組，取創作者看得懂的名字，例如「封面／代表圖」「角色代表圖」。`asset_create_group` 回傳的群組 id 照用，不自己編。縮圖用 `story.thumbnail_image_id`／`story.primary_image_id`，角色頭像用 `characters[].avatar_image_id`，都是圖庫 id，不是工作區連結 id。祕密圖維持 `link_type: "secret"`，不當縮圖或公開頭像。
- 付費 AI 生圖：不從「幫我做幾張圖」這種模糊請求開始。先列出工具或模型、張數、比例與品質、是否含重新生成、單張成本、可用餘額（工具有提供時）、整批最高總價、每張或每組的提示摘要，等使用者另外明確同意。工具無法提供價格或餘額時，不呼叫生成，改請使用者在 Tikita 介面確認或改用上傳、匯入。生成後只放進圖庫與草稿；品質不夠要再生成時，重新報價、再確認一次。
- 有中日韓文字標題的封面，先生無字底圖，文字在外部排版，不靠生圖模型拼字。
- 大量搬圖前先說明範圍與確切張數並等確認；先完成文字搬遷並請使用者確認，再搬圖。
