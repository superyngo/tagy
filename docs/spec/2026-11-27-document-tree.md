# Document tree and editing session
Status: Draft

The first spec (BACKLOG O1). It covers the **Document tree** model, **Wrap**, whitespace, **Node**
identity, row layout, and **Front matter**, all behind one test seam: the core **Session**. The
**Format**/**Lint** spec (O2) and the **Validate** spec (O3) build on this one. Decisions come
from `docs/memo/2026-11-27-brainstorm.md` and BACKLOG W1–W11; this spec restates them and does
not change them.

## Problem Statement

People who edit XML, its **Dialects** (SVG, RSS/Atom, plist, …), HTML, and Markdown either use a
text editor, which shows no structure, or a structure tool that rewrites the file: it reorders
attributes, reflows whitespace, or normalizes the markup, so a small edit produces a whole-file
diff. Each format also has its own tool with its own model. There is no single editor that shows
any of these files as one navigable tree, lets the user change it through tree operations, and
writes back exactly what was there except for what the user changed. confy does this for
key–value config files; markup has no equivalent.

## Solution

tagy parses a file in its **Source format** into a **Document tree** with only two kinds of
**Node**: **Element** and **Text node**. The user browses the tree as rows, selects a row to edit
it in a **Detail panel**, and changes the document through tree operations, including **Wrap**
and **Unwrap**. Saving writes the original source text back byte-for-byte except where the user
made an edit; whitespace the user never touched is never rewritten. The same core runs in the
terminal (TUI), the browser (web), and the desktop and mobile app (Tauri), so every host behaves
the same.

## User Stories

### Opening and saving

1. As an editor user, I want to open an XML file as a tree, so that I can see its structure at a glance.
2. As an editor user, I want to open an HTML file as the same kind of tree, so that I don't need a separate tool for HTML.
3. As an editor user, I want to open a Markdown file as the same kind of tree, so that headings, lists, and paragraphs become navigable nodes.
4. As an editor user, I want Markdown nodes named with the **CommonMark XML** vocabulary (`heading`, `paragraph`, `list`, …), so that the names are standard and predictable.
5. As an editor user, I want the **Dialect** of an XML file detected by its extension, so that an `.svg` file is treated as SVG without asking me.
6. As an editor user, I want a `.xml` file whose extension is ambiguous to be identified by its root element and namespace, so that an RSS feed and a Maven `pom.xml` are told apart.
7. As an editor user, I want saving a file I did not change to produce exactly the bytes I opened, so that opening a file in tagy is never a risk.
8. As an editor user, I want saving after an edit to change only the bytes of the nodes I edited, so that my diff shows only my change.
9. As an editor user who shares files with others, I want their indentation, line endings, quoting, and attribute order preserved, so that I don't create noise in their history.

### The two node kinds

10. As an editor user, I want every item in the tree to be either an **Element** or a **Text node**, so that the model is simple to learn.
11. As an editor user, I want an **Element** row to show its tag name and **Attributes**, so that I can identify it without opening it.
12. As an editor user, I want a **Text node** row to show its text, so that I can read content in the tree.
13. As an editor user, I want `<b class="x">world</b>` to appear as a `b` **Element** containing one **Text node**, so that text never secretly carries attributes.
14. As an editor user, I want `<span>a<b>b</b>c</span>` to appear as a `span` with three children in order, so that **Mixed content** keeps its true order.
15. As an editor user, I want a **Text node** to have no children and no **Attributes**, so that the only way to structure text is explicit.

### Wrap and Unwrap

16. As an editor user, I want to **Wrap** a selected **Text node** in a new **Element** by command, so that I can give text a tag.
17. As an editor user, I want to **Wrap** several adjacent sibling **Nodes** in one new **Element**, so that I can group them.
18. As an editor user, I want adding an **Attribute** to a **Text node** to **Wrap** it automatically, so that I don't have to wrap first and then add the attribute.
19. As an editor user of HTML, I want an automatic **Wrap** to pre-fill the tag `span`, so that the common case needs no typing.
20. As an editor user of generic XML, I want an automatic **Wrap** to leave the tag empty for me to fill, so that tagy never invents a tag my schema doesn't know.
21. As an editor user of an XML **Dialect**, I want the **Dialect** to be able to supply its own default wrap tag, so that the default fits the vocabulary.
22. As an editor user, I want to change the pre-filled wrap tag before or after the **Wrap**, so that the default is only a suggestion.
23. As an editor user of Markdown, I want actions that would need an automatic **Wrap** to be refused, so that I don't create markup Markdown cannot express.
24. As an editor user, I want to **Unwrap** an **Element**, removing it and keeping its children in place, so that I can undo a wrap or strip a tag.

### Editing

25. As an editor user, I want to edit a **Text node**'s text, so that I can change content.
26. As an editor user, I want to rename an **Element**'s tag, so that I can change its meaning.
27. As an editor user, I want to add, edit, rename, and remove **Attributes** in a key-value table in the **Detail panel**, so that attribute editing feels like confy.
28. As an editor user, I want to insert a new **Node** before, after, or inside a selected **Node**, so that I can extend the document.
29. As an editor user, I want a newly inserted **Node** to copy its siblings' indentation, so that the file still looks hand-written.
30. As an editor user, I want to delete a **Node** and its subtree, so that I can remove content.
31. As an editor user, I want an edit that would make the document invalid for its **Source format** to be refused with a message, leaving the document unchanged, so that I never save broken markup.

### Whitespace

32. As an editor user, I want whitespace-only **Text nodes** that contain a line break (indentation) hidden from the tree, so that the tree is not cluttered with layout.
33. As an editor user, I want whitespace on one line, such as the space in `<b>a</b> <i>b</i>`, shown as content, so that I can see and edit spaces that affect rendering.
34. As an editor user, I want all whitespace shown inside `<pre>` and inside `xml:space="preserve"`, so that I can see every character where whitespace is meaningful.
35. As an editor user, I want hidden whitespace still kept and written back unchanged, so that hiding is only a view.

### Node identity and collapse

36. As an editor user, I want my selection to stay on the same **Node** after an edit elsewhere, so that the cursor doesn't jump.
37. As an editor user, I want a collapsed **Node** to stay collapsed after unrelated edits, so that my view is stable while I work.
38. As an editor user, I want **Mixed content** collapsed into one row by default and element-only children expanded, so that prose reads as prose and structure reads as structure.
39. As an editor user, I want to toggle any **Node** between collapsed and expanded, so that I can override the default.
40. As an editor user, I want collapse state to reset to the default when I reopen a file, so that tagy keeps no hidden per-file state.

### Row layout and Detail panel

41. As an editor user, I want an **Element** row to show the tag and **Attributes** on one line, truncated when long, so that rows stay one line high.
42. As an editor user, I want selecting a row to open the **Detail panel** with the full **Attributes** as a key-value table, so that I can see what was truncated and edit it.
43. As an editor user, I want the same rows and **Detail panel** content in the TUI, web, and app, so that switching hosts doesn't change what I see.

### Front matter

44. As a Markdown user, I want `---` YAML or `+++` TOML **Front matter** shown as one `front_matter` **Element** with `format="yaml"` or `format="toml"`, so that it has a place in the tree.
45. As a Markdown user, I want the **Front matter** body kept as one raw **Text node** and written back unchanged, so that tagy never reformats my metadata.
46. As a Markdown user, I want to edit the raw **Front matter** text, so that I can change metadata before the key-value editor exists (W10).

### Hosts

47. As a terminal user, I want the same tree and edits in the TUI, so that I can work over SSH.
48. As a browser user, I want the same tree and edits on the web with no install, so that I can edit a file anywhere.
49. As a desktop or mobile user, I want the same tree and edits in the app, with native open and save, so that tagy works like a normal app.
50. As a maintainer, I want all hosts driven by one **Session** in the core, so that a behavior fixed once is fixed everywhere.

## Implementation Decisions

### Architecture (W11)

- `tagy-core` is pure Rust and owns parsing, the **Document tree**, lossless editing, and all
  editor state. It does no file I/O, has no C dependency, and must build for
  `wasm32-unknown-unknown`. Every core dependency must meet the same bar.
- Hosts: `tagy-tui` (ratatui, calls the core natively), `tagy-ffi` (wasm-bindgen wrapper),
  `web/` (TypeScript UI over the wasm core), `tagy-tauri` (Tauri shell over the web UI for desktop
  and mobile; adds native file I/O only, editing runs in the in-webview wasm core). Same split as
  confy.
- Hosts read files and hand the core text plus a file name; the core never opens files.

### The Session seam

- One **Session** per open document. It is the only interface hosts use and the only seam tests
  use.
- Operations, in prose:
  - Load: text and file name in → a **Session**, or a parse error. The file name drives
    **Source format** and **Dialect detection**.
  - Dispatch: one **Intent** in → the **Session** state after it, or a refusal with a reason
    and the document unchanged. Every **Intent** is atomic.
  - Snapshot: the current **View rows** plus selection and **Detail panel** content; this is
    everything a host draws.
  - Serialize: the document's current source text, which the host writes to disk.
- The **Intent** and **Snapshot** shapes are the wire contract to the wasm layer; they must be
  serializable (as confy's `Intent`/`SessionSnapshot` are through serde-wasm-bindgen).
- First-wave **Intents**: select **Node**; toggle collapse; edit text; rename tag; add, edit,
  rename, remove **Attribute**; insert **Node** (before, after, inside); delete **Node**; **Wrap**
  (one **Node** or adjacent siblings, with a tag); **Unwrap**.

### Document tree

- Two **Node** kinds only: **Element** (tag, ordered **Attributes**, ordered children) and
  **Text node** (character data, no tag, no **Attributes**, no children).
- Markdown **Nodes** use **CommonMark XML** names.
- Comments, processing instructions, CDATA sections, and doctype declarations are not **Nodes**
  in this spec; they are kept in the source text and written back unchanged (see Further Notes).

### Lossless round trip

- Load then serialize with no **Intent** returns the input byte-for-byte, for every
  **Source format** and every input the parser accepts.
- After an **Intent**, only the source span of the affected **Nodes** changes; all other bytes,
  including whitespace-only **Text nodes**, quoting, attribute order, and line endings, are
  unchanged.
- Whitespace is never rewritten on save. Rewriting indentation is **Format** (O2), which runs only
  on user command.
- HTML input the parser had to repair (unclosed or misplaced tags) still loads; where the repaired
  tree cannot be serialized back to the original bytes, the place is reported by **Lint** (O2).

### Wrap and Unwrap (W6)

- **Wrap** inserts one new **Element** around one **Node** or a run of adjacent siblings;
  **Unwrap** removes an **Element** and splices its children into its parent at its position.
- **Wrap** runs on an explicit **Intent**, or automatically when an **Intent** needs an
  **Element** but targets a **Text node** (first wave: adding an **Attribute**).
- Automatic wrap tag: HTML `span`; generic XML empty (the user must fill it before the edit
  commits); a **Dialect** may supply its own default. The tag is always editable.
- Markdown refuses any **Intent** that would need an automatic **Wrap**.

### Whitespace (W7)

- Whitespace-only **Text nodes** stay in the model.
- A whitespace-only **Text node** that contains a line break produces no **View row**. One without
  a line break is content and produces a row.
- Inside `<pre>` and inside an **Element** with `xml:space="preserve"` (inherited by
  descendants), every whitespace-only **Text node** produces a row.
- An inserted **Node** takes the indentation of its preceding sibling, or of its following sibling
  when it is first; under an empty parent, the parent's indentation plus the unit inferred from
  the file.

### Node identity (W8)

- Each **Node** gets an internal ID at load. IDs are stable for the **Session**: an **Intent**
  never changes the ID of a **Node** it did not create or delete; **Wrap** and **Unwrap** keep the
  IDs of the wrapped children.
- IDs are not written to the file and not persisted across **Sessions**.
- Selection and collapse state are keyed by ID.

### View rows and collapse (W5 provisional, W8, W9)

- Each **View row** carries the **Node** ID, depth, kind, and display text. An **Element** row
  shows the tag and **Attributes** on one line; the host truncates to its width.
- Default collapse: an **Element** whose children include a non-whitespace **Text node** is
  collapsed into one row; an element-only **Element** is expanded. The user can toggle any
  **Node**; the toggle lasts for the **Session**. Collapse is a view state and never changes the
  document. This rule is provisional (W5) and may be retuned after hands-on use.
- The **Detail panel** for an **Element** lists its **Attributes** as an ordered key-value table;
  for a **Text node**, its full text.

### Front matter

- A Markdown file that starts with a `---` or `+++` fenced block gets one `front_matter`
  **Element** as the first child of `document`, with `format="yaml"` or `format="toml"` and one
  raw **Text node** holding the block body.
- The fences and body are written back unchanged unless the user edits the body text.
- **Format** and **Lint** never touch it (O2). Key-value editing is later (W10).

### Dialects (W3)

- **Dialect detection**: file extension first; when the extension is ambiguous (e.g. `.xml`),
  root element name and namespace.
- In this spec a **Dialect** supplies only a default automatic wrap tag. The full extension point
  (e.g. SVG preview) is designed later.

## Testing Decisions

- A good test drives the **Session** only: load text, dispatch **Intents**, then assert on the
  **Snapshot** and the serialized text. Tests never reach into parser, tree, or span internals, so
  the internals can be replaced without touching tests.
- One seam: `tagy-core`'s **Session**. Hosts are thin renderers of the **Snapshot** and get no
  seam of their own in this spec.
- Test groups:
  - Round trip: fixture files per **Source format** and **Dialect** (including odd whitespace,
    CRLF, mixed quoting, **Front matter**, entities) load and serialize byte-for-byte.
  - Property-based round trip: generated documents survive load → serialize unchanged, and a
    random edit changes only the edited **Node**'s span.
  - **Intents**: each first-wave **Intent** on representative documents, asserting the serialized
    text and **View rows**; refused **Intents** leave serialize output identical.
  - **Wrap**/**Unwrap**: manual and automatic, default tags per **Source format**, Markdown
    refusal, IDs kept.
  - Whitespace visibility: line-break whitespace hidden, same-line spaces shown, `<pre>` and
    `xml:space="preserve"` (including inheritance) shown.
  - Identity: selection and collapse survive a sequence of unrelated **Intents**.
  - wasm gate: `tagy-core` builds for `wasm32-unknown-unknown` in CI, and a check that the core
    has no file-system access.
- Prior art: confy's `confy-core` tests — `roundtrip.rs`, `roundtrip_proptest.rs`,
  `session_headless.rs`, `hostile_input.rs`, and `no_fs_gate.rs`.

## Out of Scope

- Conversion between **Source formats**; **XML export** gets its own spec (W1).
- **Format**, **Lint** (O2) and **Validate** (O3).
- Package formats: OOXML, EPUB, ODF (W2).
- HTML-like templates: Vue, Svelte, JSX, BBCode (W4).
- Key-value editing of **Front matter** (W10).
- The **Dialect** extension point beyond a default wrap tag, including SVG preview (W3).
- Inline `<svg>` and MathML inside HTML (foreign content).
- Persisting collapse state or any other per-file state across **Sessions**.

## Further Notes

Not decided by this spec; each needs a decision before or during implementation:

- Parsers: which XML, HTML, and Markdown parsers, and how each keeps source spans for the lossless
  round trip (confy uses `rowan` for its lossless tree). HTML5 parsers and common Markdown
  parsers do not keep exact source text; this is the main technical risk.
- Comments, processing instructions, CDATA, and doctype: kept as source text here; whether they
  become visible rows is open.
- Undo and redo, move or reorder **Nodes**, copy and paste, search and filter.
- Error handling when a file does not parse at all (XML not well-formed): refuse to load, or load
  as raw text.
- Large-file performance.
