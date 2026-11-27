# Backlog
Status: In progress

The one living record of open work. Rows move to Done with the commit that closed them and are
never deleted. Evidence is file + symbol, never a line number. `Verified` is the date the row was
last checked against the tree — not when it was opened.

## Open

| ID | Opened | Verified | Pri | Finding | Evidence | Effort | Acceptance |
|---|---|---|---|---|---|---|---|
| O1 | 2026-11-27 | 2026-11-27 | P1 | Decide the **Text node** model in **Mixed content** (display is provisional, W5) | `docs/reference/glossary.md` **Mixed content** | S | Decision written into the first spec in `docs/spec/` |

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
| W5 Provisional 2026-11-27: **Mixed content** display — granularity defaults by auto rule (text among children → collapsed; element-only → expanded); user can toggle per **Node** and the state is remembered | Needs hands-on use to tune | First usable tree UI — re-evaluate after real use | 2026-11-27 |

## Done

| ID | Finding | Closed by |
|---|---|---|
