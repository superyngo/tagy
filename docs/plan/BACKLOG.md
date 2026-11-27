# Backlog
Status: In progress

The one living record of open work. Rows move to Done with the commit that closed them and are
never deleted. Evidence is file + symbol, never a line number. `Verified` is the date the row was
last checked against the tree — not when it was opened.

## Open

| ID | Opened | Verified | Pri | Finding | Evidence | Effort | Acceptance |
|---|---|---|---|---|---|---|---|
| O1 | 2026-11-27 | 2026-11-27 | P1 | Decide how **Mixed content** appears in the tree: one **Node** per text run, or collapsed into one editable line | `docs/reference/glossary.md` **Mixed content** | S | Decision written into the first spec in `docs/spec/` |

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
| W2 OOXML as a source format (zip package of XML parts) | XML, HTML, Markdown first | XML editing ships | 2026-11-27 |

## Done

| ID | Finding | Closed by |
|---|---|---|
