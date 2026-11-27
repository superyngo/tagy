# Glossary

Canonical vocabulary for tagy. Code identifiers, UI strings, commit messages, and every other
document use these terms. A new term gets its entry here in the same commit.

**Source format**:
The markup language a file is written in: XML, HTML, or Markdown. Package formats (OOXML, then
EPUB and ODF) are planned later. tagy edits a file in its own source format and never converts
between them.
_Avoid_: Language, file type.

**Dialect**:
A vocabulary on top of a **Source format**, chiefly XML: SVG, RSS/Atom, MathML, XAML, plist,
and so on. A dialect shares its source format's parser and **Document tree**; it only adds
features of its own (e.g. an SVG preview) through an extensible layer.
_Avoid_: Format, schema, flavor.

**Dialect detection**:
Choosing a file's **Dialect**: by file extension first, then by root element name and
namespace when the extension is ambiguous (e.g. `.xml`).
_Avoid_: Sniffing, format detection.

**Document tree**:
The tree tagy builds from one file, shared by every **Source format**: one parse, operation, and
UI model for all of them.
_Avoid_: DOM, AST.

**Node**:
Any single item in the **Document tree**; the umbrella term for everything the user navigates
and operates on.
_Avoid_: Entry, item.

**Element**:
A **Node** with a tag name, an ordered set of **Attributes**, and ordered child **Nodes**. The
markup counterpart of confy's key–value pair.
_Avoid_: Tag (the name only), key.

**Attribute**:
A name–value pair on an **Element**.
_Avoid_: Property, field.

**Text node**:
A **Node** holding character data between or inside **Elements**. It has no tag, no
**Attributes**, and no children. **Element** and Text node are the only two kinds of **Node**:
text that carries attributes, such as `<b class="x">world</b>`, is an **Element** whose only
child is a Text node, not a third kind.
_Avoid_: String, value, implicit text, explicit text.

**Validate**:
Checking a document's structure against a schema (e.g. DTD, XSD) and reporting violations.
Separate from **Lint**, which checks style; results are shown in the same place and run on the
same schedule. Never blocks editing or saving.
_Avoid_: Lint, verify, check.

**Wrap**:
Putting a new **Element** around a **Text node** (or other **Nodes**); the only way text gains a
tag or **Attributes**. **Unwrap** is the inverse: remove the **Element** and keep its children.
Wrap runs on a user command, or automatically when the user does something only an **Element**
allows (e.g. adds an **Attribute** to a **Text node**). An automatic wrap pre-fills a tag from the
**Source format** or **Dialect** (empty for generic XML), which the user can change; Markdown does
not allow automatic wrap.
_Avoid_: Promote, lift.

**Format**:
Rewriting a document's indentation and layout on explicit user command. Never runs on save;
saving writes whitespace back unchanged.
_Avoid_: Beautify, pretty-print, auto-format.

**Front matter**:
The metadata block at the top of a Markdown file (`---` YAML or `+++` TOML). Not part of
CommonMark; tagy keeps it as one `front_matter` **Element** with a `format` attribute and one raw
**Text node**, written back unchanged.
_Avoid_: Metadata block, header, YAML header.

**Lint**:
Checking a document and reporting problems (e.g. inconsistent indentation) without changing it.
Runs after each committed edit, paused while an edit is open; never blocks editing or saving.
_Avoid_: Validate (reserved for schema checks), fix.

**Mixed content**:
An **Element** whose children interleave **Text nodes** and **Elements**, as in
`<p>Hello <b>world</b>!</p>`. Order and whitespace may be significant. How the tree shows it is
provisional (W5 in `docs/plan/BACKLOG.md`).
_Avoid_: —

**XML export**:
Writing any **Document tree** out as standard XML: HTML as XHTML, Markdown in the **CommonMark
XML** vocabulary. The only output besides saving in the **Source format**.
_Avoid_: Conversion, translation.

**CommonMark XML**:
The XML vocabulary defined by the CommonMark spec (`cmark -t xml`): `document`, `heading`,
`paragraph`, `list`, and so on. tagy names Markdown **Nodes** with it.
_Avoid_: Markdown AST.

**Session**:
One open document in `tagy-core`: its **Document tree** plus all editor state (selection,
collapse). The only interface hosts (TUI, web, app) use, and the one seam tests use.
_Avoid_: Editor, buffer, workspace.

**Intent**:
One user action dispatched to a **Session** (e.g. **Wrap**, rename tag, add **Attribute**).
Atomic: it applies fully or is refused and leaves the document unchanged.
_Avoid_: Command, mutation, action.

**Snapshot**:
Everything a host draws for a **Session** at one moment: the **View rows**, the selection, and
the **Detail panel** content. Serializable, so it crosses the wasm boundary.
_Avoid_: State, view model.

**View row**:
One line of the tree as shown: a **Node** ID, depth, kind, and display text. A hidden
whitespace **Text node** has none; a collapsed **Element** is one row.
_Avoid_: Line, item.

**Detail panel**:
The pane that shows the selected **Node** in full: an **Element**'s **Attributes** as a
key-value table, or a **Text node**'s whole text.
_Avoid_: Inspector, properties pane.
