# Changelog

## [Unreleased]

### 2026-11-27
- docs: draft O1 spec — document tree and editing session; glossary Session, Intent, Snapshot, View row, Detail panel
- docs: Lint and Validate results display and live timing decided; O2 discussion closed
- docs: architecture follows confy — pure-Rust core shared by TUI, web and Tauri app (W11)
- docs: Validate scope — DTD self-implemented, XSD via pure-Rust crate after wasm spike; no libxml2, no RELAX NG
- docs: Lint first-wave checks decided; new O3 Validate (XML only); libxml2 cross-platform findings in memo
- docs: O2 scope, indent source, Format rules decided; Markdown front matter as one raw element (W10 for key-value editing)
- docs: memo records order — discuss O2 first, then two specs (O1, O2)
- docs: memo overview lists W7–W9 under decided; W5 toggle state is session-only (W8)
- docs: decide whitespace text nodes, node identity, row layout (W7–W9); plan Format and Lint (O2)
- docs: memo overview of decisions so far; backlog W7–W9 parking-lot items
- docs: decide automatic Wrap tag rule (format/dialect default, editable; none in Markdown)
- docs: decide Wrap timing (manual and automatic)
- docs: decide two-node model (Element, Text node) and Wrap/Unwrap; wrap timing still open
- docs: record provisional mixed-content display rule (auto default, manual toggle remembered)
- docs: record dialect scope (SVG and XML dialects with XML, extensible dialect layer, detection order), package formats, templates out of scope
- docs: initialize repo-docs skeleton (dev profile), glossary, backlog, MIT license
