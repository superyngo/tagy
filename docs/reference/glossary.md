# Glossary

Canonical vocabulary for tagy. Code identifiers, UI strings, commit messages, and every other
document use these terms. A new term gets its entry here in the same commit.

**Source format**:
The markup language a file is written in: XML, HTML, or Markdown. OOXML is a planned later
source format. tagy edits a file in its own source format and never converts between them.
_Avoid_: Language, file type.

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
A **Node** holding character data between or inside **Elements**.
_Avoid_: String, value.

**Mixed content**:
An **Element** whose children interleave **Text nodes** and **Elements**, as in
`<p>Hello <b>world</b>!</p>`. Order and whitespace may be significant. How the tree shows it is
an open question (see `docs/plan/BACKLOG.md`).
_Avoid_: —

**XML export**:
Writing any **Document tree** out as standard XML: HTML as XHTML, Markdown in the **CommonMark
XML** vocabulary. The only output besides saving in the **Source format**.
_Avoid_: Conversion, translation.

**CommonMark XML**:
The XML vocabulary defined by the CommonMark spec (`cmark -t xml`): `document`, `heading`,
`paragraph`, `list`, and so on. tagy names Markdown **Nodes** with it.
_Avoid_: Markdown AST.
