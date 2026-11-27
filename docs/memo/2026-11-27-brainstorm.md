# 2026-11-27 起始發想

## 目前總覽（截至 2026-11-27）

下方各節是當天的思考過程，依時間順序保留；過程中寫的「尚未決定」「傾向」已被後面的定案取代，以本節為準。正式紀錄：詞彙見 `../reference/glossary.md`，決定與待辦見 `../plan/BACKLOG.md`（W1–W9、O1）。

### 已定案

- **定位**：confy 的姊妹作，樹狀瀏覽＋無損編輯；節點是 tag + attributes，不是 key-value。
- **格式範圍**：先做 XML（含 SVG 等方言）與 HTML；Markdown 收進同一棵樹。之後：OOXML → EPUB、ODF（zip 包 XML，共用打包層）。不做：HTML 模板（Vue、Svelte、JSX、BBCode）。
- **不做格式轉換**：只做統一編輯（共用樹解析、操作邏輯、介面）；任何樹都可輸出為標準 XML（HTML → XHTML）。
- **Markdown 命名**：沿用 CommonMark XML（`cmark -t xml`）。
- **方言**：共用 XML 解析器；方言功能是可擴充的一層（例如 SVG 預覽）。偵測順序：副檔名 → 根元素 → namespace。
- **節點模型**：只有兩種節點——element、文字節點。文字節點沒有 tag、沒有 attributes、沒有子節點。
  - `<b class="x">世界</b>` 是 `<b>` 包著一個文字節點。
  - `<span>外圍<b>世界</b>天地</span>` 是 span 底下三個並排子節點（混合內容）。
- **包裹（wrap）／拆包（unwrap）**：文字要有 tag 或 attributes，唯一方式是包裹。
  - 觸發：手動指令，或自動（對文字節點做只有 element 能做的事，例如加 attribute）。
  - 自動包裹的 tag：格式或方言給預設值，使用者可改。HTML 預設 `<span>`；通用 XML 留空由使用者填，方言可提供預設；Markdown 不允許自動包裹。

### 暫定（需實際操作後調整）

- **混合內容的顯示**：預設自動判斷（子節點有文字就收合成一行，全是 element 就展開）；使用者可手動切換並記住狀態。收合與展開只是畫面，不改模型。

### 待處理

- 把節點模型與包裹規則寫進第一份 spec（BACKLOG O1）。

### 之後會碰到（停車場）

- XML 縮排空白本身是文字節點，樹上怎麼處理（W7）。
- 跨工作階段記住收合狀態，需要穩定辨識「同一個節點」（W8）。
- 樹狀編輯器畫面上 tag 與 attributes 如何拆分、如何合成顯示（W9）。

---

## 思考過程

## 出發點

- 通用標註語言解析編輯器：XML / OOXML / HTML / XHTML / Markdown。
- 參考 `~/repos/wenv`、`~/repos/confy` 的樹狀解析與編輯。
- 與 JSON 等 key-value 結構不同：節點是 tag + attributes。

## 五種格式其實三類

- 嚴格樹：XML、XHTML。
- 寬鬆樹：HTML（HTML5 解析規則把任何輸入變成確定的樹）。
- 其他：OOXML 是 zip 裡多個 XML（像專案資料夾）；Markdown 沒有 tag，結構是隱式的。

## 範圍

- 先做 XML、HTML；Markdown 的隱式結構收斂進同一棵樹。
- OOXML 列為之後延伸。

## 轉換的討論

- Markdown ↔ HTML 自然（Markdown 是照 HTML 子集設計）。
- HTML → XML 一定可行（輸出 XHTML），代價是原始寫法被正規化。
- XML → HTML / Markdown：同詞彙換語法可行；自訂 tag 跨詞彙需要對照規則。
- 分界不是方向，而是「同詞彙換語法」vs「跨詞彙」。
- confy 的核心是無損往返；轉換本質有損，兩者是不同模式。

## 定案（2026-11-27，已確認）

- 不提供格式間轉換。
- 提供統一的編輯：共用樹解析、操作邏輯與介面。
- 任意樹都可輸出為標準 XML（HTML → XHTML）。
- Markdown 節點沿用 CommonMark XML 命名（`cmark -t xml`）。

## 尚未決定

- 混合內容（`<p>你好 <b>世界</b>！</p>`）在樹中如何呈現：每段文字展開成節點，還是收合成一行整段編輯？

## 追加定案（2026-11-27，已確認）

- SVG 及其他 XML 方言（RSS/Atom、MathML、XAML、plist、pom.xml、Android layout、GPX/KML、DocBook…）跟 XML 一起做，共用 XML 解析器。
- 針對方言提供彈性擴展（例如 SVG 預覽）。
- 方言偵測：副檔名優先，再看根元素和 namespace（例如 `.xml` 分不出 RSS 還是 Maven）。
- EPUB、ODF 列入 OOXML 後續規劃（同為 zip 包多個 XML）。
- HTML 模板（Vue、Svelte、JSX、BBCode）先不做。

## 混合內容的顯示（暫定，2026-11-27）

- 自動判斷與手動切換並用：預設自動（子節點有文字就收合，全是 element 就展開）。
- 使用者可手動切換並記住狀態。
- 保留調整空間，需實際上手操作後再定。

## 文字節點模型（定案，2026-11-27）

- 模型只有兩種節點：element 與文字節點（隱式，不能有 attribute、不能有子節點）。
- `<b class="x">世界</b>` 是 `<b>` 裡包著一個文字節點，不是第三種節點。
- 提升就是包裹（wrap）；反向為拆包（unwrap）。
- 包裹時機：手動 vs 由動作觸發，差別在是否必須先選好 tag；傾向手動，尚未定案。

## 包裹時機（定案，2026-11-27）

- 文字節點維持不能有子節點（`<span>外圍<b>世界</b>天地</span>` 是 span 底下三個並排子節點）。
- 包裹 = 手動觸發 + 自動觸發（對文字節點做只有 element 能做的事，例如加 attribute）。
- 自動包裹的 tag：格式或方言給預設值，使用者可改。
  - HTML 預設 `<span>`。
  - 通用 XML 留空讓使用者填；方言可提供預設值。
  - Markdown 不允許自動包裹（文字不能帶 attribute）。
