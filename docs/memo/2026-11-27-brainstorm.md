# 2026-11-27 起始發想

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
