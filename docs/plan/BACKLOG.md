# Backlog
Status: In progress

The one living record of open work. Rows move to Done with the commit that closed them and are
never deleted. Evidence is file + symbol, never a line number. `Verified` is the date the row was
last checked against the tree — not when it was opened.

## Open

| ID | Opened | Verified | Pri | Finding | Evidence | Effort | Acceptance |
|---|---|---|---|---|---|---|---|
| O1 | 2026-11-27 | 2026-11-27 | P1 | Write the node model, **Wrap** rules, whitespace, node identity, row layout (W6–W9) and **Front matter** into the first spec; display provisional (W5) | `docs/reference/glossary.md` **Mixed content** | S | Decision written into the first spec in `docs/spec/` |
| O2 | 2026-11-27 | 2026-11-27 | P3 | Plan **Format** and **Lint** as separate features: **Format** runs only on user command, **Lint** only reports; neither runs on save | `docs/reference/glossary.md` **Format**, **Lint** | M | Scope written into a spec in `docs/spec/` |
| O3 | 2026-11-27 | 2026-11-27 | P1 | Plan **Validate** as its own feature: schema checks of structure, results shown with **Lint**; first wave XML only: DTD (self-implemented) and XSD (pure-Rust crate, chosen after a wasm build spike of `xsd-schema` vs `xmlschema`); no libxml2, no RELAX NG | `docs/memo/2026-11-27-brainstorm.md` Validate section | M | Scope written into its own spec in `docs/spec/` |

## Pending verification

Landed, but the check needs a platform or pipeline not available locally.

| Item | Closed by | Verifies when | Fallback |
|---|---|---|---|

## Awaiting external

Blocked on a person or third party. **Not counted as open** — no amount of local work closes it.

| Item | Blocked on | Ready when |
|---|---|---|

## Watching

Known, deliberately not scheduled — each with the reason it is not a defect *yet*, the **trigger**
that would promote it to Open, and the date it was last re-read.

| Item | Why not now | Trigger | Re-read |
|---|---|---|---|
| W1 Scope decided 2026-11-27: no conversion between source formats; one shared tree parse / edit logic / UI; any tree has an **XML export**; Markdown nodes use **CommonMark XML** names | Exploration stage; nothing builds on it yet, so not an ADR (repo-docs 16) | First spec lands — restate there; ADR once a persisted format or public interface depends on it | 2026-11-27 |
| W2 Package formats — OOXML first, then EPUB and ODF (zip packages of XML parts, one shared packaging layer) | XML, HTML, Markdown first | XML editing ships | 2026-11-27 |
| W3 **Dialects** decided 2026-11-27: SVG and other XML dialects (RSS/Atom, MathML, XAML, plist, …) ship with XML on the one XML parser; per-dialect features are an extensible layer; **Dialect detection** by file extension first, then root element and namespace | Exploration stage; extension API not designed yet | First spec lands — design the dialect extension point there | 2026-11-27 |
| W4 HTML-like templates (Vue, Svelte, JSX, BBCode) out of scope | Each needs its own parser; would slow the main line | Explicit owner request | 2026-11-27 |
| W5 Provisional 2026-11-27: **Mixed content** display — granularity defaults by auto rule (text among children → collapsed; element-only → expanded); user can toggle per **Node**; the state lasts for the editing session (W8) | Needs hands-on use to tune | First usable tree UI — re-evaluate after real use | 2026-11-27 |
| W6 Decided 2026-11-27: two **Node** kinds only (**Element**, **Text node**); text gains a tag or attributes only by **Wrap**. **Wrap** is both manual (user command) and automatic (an action only an **Element** allows, e.g. adding an **Attribute** to a **Text node**). Automatic **Wrap** tag: default from the **Source format** or **Dialect** (HTML: `span`; generic XML: empty, user fills it; a **Dialect** may supply one), always editable by the user; not allowed in Markdown | Exploration stage; not an ADR (repo-docs 16) | First spec lands — restate there | 2026-11-27 |
| W7 Decided 2026-11-27: whitespace-only **Text nodes** stay in the model and are written back unchanged on save. The tree hides only those containing a line break (indentation); same-line spaces such as `<b>a</b> <i>b</i>` show as content. Always shown inside `<pre>` and `xml:space="preserve"`. A new **Node** copies its siblings' indentation | Exploration stage; not an ADR (repo-docs 16) | First spec lands — restate there | 2026-11-27 |
| W8 Decided 2026-11-27: each **Node** gets an internal ID at parse, stable for the editing session. Collapse state (W5) is not persisted across sessions; reopening falls back to the automatic default | Exploration stage; not an ADR (repo-docs 16) | First spec lands — restate there | 2026-11-27 |
| W9 Decided 2026-11-27: an **Element** row shows tag and **Attributes** on one line, truncated when long; selecting it opens a detail panel that edits **Attributes** as a key-value table (after confy) | Exploration stage; not an ADR (repo-docs 16) | First spec lands — restate there | 2026-11-27 |
| W10 Edit **Front matter** in the detail panel with a confy-style key-value editor; the tree keeps it as one **Element** with a raw **Text node** | Decided 2026-11-27 as a later extension, not first wave | Markdown editing usable and the detail panel (W9) exists | 2026-11-27 |

## Done

| ID | Finding | Closed by |
|---|---|---|
