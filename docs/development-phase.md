# JSON Visual Editor — Development Phases (Draft v0.1)

> แผนแบ่ง phase จาก `requirements-checklist.md` (v0.2) เรียงตามลำดับที่ต้องทำก่อนหลัง
> ทุก requirement ที่ติ๊ก `[x]` อยู่ใน phase ใด phase หนึ่งเพียงที่เดียว (ดูตาราง coverage ท้ายไฟล์)
> แต่ละ phase จบด้วยแอปที่รันได้และทดสอบได้ ข้อที่เป็น "later, not v1" (C-09, P-03) ไม่อยู่ในแผนนี้
> การตัดสินใจใหม่ระหว่างทำ ให้เขียนต่อท้าย `decision-log.md`

**Order:** Phase 0 → 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9
Phases 6 (node operations) and 7 (rich preview) only depend on Phase 5 and could swap order.

---

## Phase 0 — Foundation & technical spike

**Goal:** prove the risky parts before building UI on top of them.

**Requirements:** P-01, C-05, N-02, N-03, N-06, N-05 (test setup), S-03 (bundle fonts), I-04 (l10n scaffold)

**Work**
- [ ] Create the Flutter macOS project, folder structure, lint rules, CI-free local test command.
- [ ] **Lossless JSON document model**: ordered list of entries (not a Dart `Map`), keeps duplicate keys, keeps numbers as their original text (no rounding), source positions (line/column) for every node.
- [ ] Strict JSON parser + serializer on top of that model (UTF-8, BOM handled), with error line/column.
- [ ] Round-trip tests: parse → serialize gives identical output for formatted input; big numbers / decimals / duplicate keys / Thai text survive.
- [ ] **Performance spike with a 50 MB file**: parse time in a background isolate, memory use, and a quick virtualized tree list. Pick the code editor widget (e.g. `re_editor` or `flutter_code_editor`) by testing it with the same file.
- [ ] Bundle IBM Plex Sans, IBM Plex Mono, IBM Plex Sans Thai Looped.
- [ ] Set up `flutter_localizations` + ARB files (en, th) and a theme token structure (light/dark) so every later phase uses them from day one.

**Done when:** model tests pass, a 50 MB file parses in the background without freezing the UI, editor widget chosen and recorded in `decision-log.md`.

---

## Phase 1 — App shell & file handling

**Goal:** an app window that opens, shows and saves a JSON file (as plain text for now).

**Requirements:** G-01, G-02, G-04, G-05, G-06, G-07, G-08 (validation status + encoding), G-09, P-06

**Work**
- [ ] Material 3 app with the custom toolbar row integrated into the macOS title bar (traffic lights on the left), pill-shaped icon buttons. Buttons are placeholders until their phase.
- [ ] Document title / file name under the toolbar; unsaved indicator `•`.
- [ ] Native macOS menu bar: File, Edit, View, Window, Help.
- [ ] New, Open, Save, Save As, Close, Recent files; "Save changes?" prompt on close.
- [ ] Drag a `.json` file onto the window to open it.
- [ ] One window per file: opening another file opens a new window; an already-open file brings its window to the front.
- [ ] Status bar skeleton (validation status, encoding).
- [ ] macOS (`Cmd`) shortcuts for all file commands.

**Done when:** can open, edit as plain text, save, and open several files in separate windows.

---

## Phase 2 — Code mode

**Goal:** a complete text editor for JSON.

**Requirements:** E-01, E-02, E-03, E-04, E-05, E-06, E-07, E-08, E-09, E-10, E-11, E-12, E-13, S-02 (default font only)

**Work**
- [ ] Syntax highlighting (key, string, number, boolean, null, punctuation) using One Light / One Dark colors.
- [ ] Line numbers, code folding for `{}` / `[]`.
- [ ] Live validation: underline errors, message on hover and in the status bar (line/column).
- [ ] Word wrap toggle.
- [ ] Bracket matching, auto-close `{ [ "`.
- [ ] Format / Pretty-print and Minify commands.
- [ ] Key auto-complete: sibling keys at the same path first, then all keys in the file.
- [ ] Find / Replace with optional regex (toolbar Search button + `Cmd+F` / `Cmd+H`).
- [ ] Normal text copy / cut / paste.
- [ ] Code font defaults to IBM Plex Mono.

**Done when:** a 50 MB file is usable in Code mode (scroll, type, fold, search) and all E items work.

---

## Phase 3 — Visual mode (view only)

**Goal:** show any JSON as the node tree from the mockup, read-only.

**Requirements:** V-01, V-02, V-03, V-04, V-05, V-06, V-07, V-08, V-10, V-20, V-21, V-22, V-30, V-34, V-62, S-04 (behaviour)

**Work**
- [ ] Virtualized tree list (only visible rows are built) so 50 MB files scroll smoothly.
- [ ] Node row: chevron (containers) → type icon → key : value → flexible space → child count `{ n }` / `[ n ]` → `×`.
- [ ] Guide lines, rounded row cards, values aligned in a second column, array indexes as keys.
- [ ] Type icons and colors for Object, Array, String, Number, Boolean, Null.
- [ ] Expand / collapse one node, Expand all, Collapse all, Expand to level N, Option-click for a whole subtree.
- [ ] Single-line vs multi-line (up to 10 lines + scrollbar) driven by the S-04 setting; single-click switches one node.
- [ ] Word wrap in Visual mode (applies to multi-line values; single-line values stay truncated).
- [ ] Keyboard navigation: arrows move selection, `←/→` collapse/expand.
- [ ] Panel resize handle (V-08).

**Done when:** the mockup's example JSON looks like the mockup (with One Light/Dark colors) and a 50 MB file scrolls smoothly.

---

## Phase 4 — Switching between modes

**Goal:** one document, two modes, kept in sync.

**Requirements:** C-01, C-02, C-03, C-04, C-07, C-08, G-03 (`Code | Visual` switch), G-08 (cursor line/column + node path)

**Work**
- [ ] `Code | Visual` segmented toolbar button and `Cmd+E`.
- [ ] Both modes read and write the same document model.
- [ ] Block switching to Visual when the text is invalid; show line/column + message.
- [ ] Keep selection and scroll position across switches (cursor ↔ selected node via source positions).
- [ ] Remember the last mode per file.
- [ ] Status bar shows cursor line/column in Code mode and node path in Visual mode.

**Done when:** edit text → switch → tree shows the change at the same place, and back again.

---

## Phase 5 — Visual mode editing

**Goal:** edit values and structure directly in the tree, with one undo history for both modes.

**Requirements:** V-11, V-12, V-13, V-31, V-32, V-33, V-35, V-40, V-41, V-42, V-43, V-44, V-45, V-46, C-06, C-10

**Work**
- [ ] Click type icon → type dropdown (Object, Array, divider, String, Number, Boolean, Null); convert value or reset to default.
- [ ] Double-click key → rename; double-click value → edit; `Enter` confirms, `Esc` cancels; reject invalid input with a message.
- [ ] Delete with `×` or `Delete/Backspace`, no confirm dialog.
- [ ] `+ ADD` row and inline add form (Name, type, Value, ✓, ×); Value hidden for Object/Array/Null; Name hidden in arrays; Name auto-complete.
- [ ] Duplicate key warning (add / rename) that still allows the change.
- [ ] Unified undo / redo across both modes.
- [ ] Re-format the document (indent per S-07) after a Visual edit; keep original formatting when only Code mode was used.

**Done when:** every edit in the tree is reflected in Code mode, and undo/redo works across mode switches.

---

## Phase 6 — Node operations

**Goal:** move and copy nodes in Visual mode.

**Requirements:** E-20, E-21, E-22, E-23, E-24, E-25, E-26, V-60, V-61

**Work**
- [ ] Single selection only.
- [ ] Drag and drop one node with an insertion line; drop before, after, or inside a container.
- [ ] Copy / Cut / Paste before / Paste after / Duplicate; clipboard holds JSON text (works with other apps).
- [ ] Duplicate key warning on paste and drag.
- [ ] Right-click context menu: Copy, Cut, Paste before, Paste after, Duplicate, Delete, Add child, Change type, Copy path.

**Done when:** all operations work and are undoable.

---

## Phase 7 — Rich value preview

**Goal:** preview images, HTML and Markdown inside string values.

**Requirements:** V-50, V-51, V-52, V-53, V-54, V-55

**Work**
- [ ] Background scan on file open for base64 images (`data:image/...;base64,` and raw base64 by magic bytes); thumbnail in the row.
- [ ] Image popup with size, MIME type, dimensions.
- [ ] HTML / Markdown auto-detect + manual "Open as HTML / Markdown / Plain text".
- [ ] Popup viewer: rendered preview by default, toggle to source edit with highlighting, saves back with correct escaping.

**Done when:** a file with base64 images and HTML/Markdown strings opens without delay and all previews work.

---

## Phase 8 — Settings, themes, favorites

**Goal:** the gear menu and remaining toolbar buttons.

**Requirements:** S-01, S-02 (font picker), S-05, S-06, S-07, S-08, S-09, G-03 (Star, Hamburger, `+` dropdown, Folder, Contrast, Gear)

**Work**
- [ ] Settings screen (gear, top right).
- [ ] One Light / One Dark / Follow system; contrast button as quick toggle.
- [ ] Pick any installed monospace font for Code mode.
- [ ] Word wrap default, font size / zoom (`Cmd +/-`), save indentation (2 / 4 / tab), language.
- [ ] Settings persist between launches.
- [ ] Star = add/remove favorite; Hamburger = sidebar with favorites and recent files.
- [ ] `+` split button: New file / New from template.

**Done when:** every setting changes the app immediately and survives a restart.

---

## Phase 9 — Thai/English, macOS integration, release

**Goal:** finish language support, make it the default `.json` app, and harden for daily use.

**Requirements:** I-01, I-02, I-03, P-02, P-04, P-05, N-01 (final check), N-04

**Work**
- [ ] All UI text translated (Thai / English); default follows macOS, can switch in Settings without restart.
- [ ] Thai rendering check: vowel/tone marks, line breaking in wrapped values.
- [ ] Register for `.json` in Info.plist (`CFBundleDocumentTypes`, UTI `public.json`) so it can be the default app.
- [ ] Open from Finder (double-click, Open With, command line) into a new window of the running app.
- [ ] Local release build (no signing / notarization).
- [ ] Final 50 MB performance check across all features; accessibility pass (keyboard-only use, contrast in both themes).

**Done when:** the app can be set as the default `.json` editor on your Mac and handles a 50 MB file in both modes.

---

## Assumptions (please correct if wrong)

- Shortcuts written as `Cmd/Ctrl` in the checklist (C-02, E-12, S-06) are `Cmd` only, since v1 is macOS only.
- "New from template" (G-03) in v1 offers simple templates only (empty object `{}` and empty array `[]`).
- Word wrap in Visual mode applies to multi-line values; single-line values stay truncated.
- UI text goes into ARB files from Phase 1 onward; Phase 9 completes and checks the Thai translation.

---

## Coverage (requirement → phase)

| Section | Phase 0 | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 | Phase 6 | Phase 7 | Phase 8 | Phase 9 |
|---|---|---|---|---|---|---|---|---|---|---|
| Core (C) | C-05 | | | | C-01–C-04, C-07, C-08 | C-06, C-10 | | | | |
| Code (E) | | | E-01–E-13 | | | | E-20–E-26 | | | |
| Visual (V) | | | | V-01–V-08, V-10, V-20–V-22, V-30, V-34, V-62 | | V-11–V-13, V-31–V-33, V-35, V-40–V-46 | V-60, V-61 | V-50–V-55 | | |
| General (G) | | G-01, G-02, G-04–G-07, G-09 | | | G-03 (mode switch), G-08 (cursor/path) | | | | G-03 (other buttons) | |
| Settings (S) | S-03 | | S-02 (default) | S-04 | | | | | S-01, S-02 (picker), S-05–S-09 | |
| Platform (P) | P-01 | P-06 | | | | | | | | P-02, P-04, P-05 |
| i18n (I) | I-04 | | | | | | | | | I-01–I-03 |
| Non-functional (N) | N-02, N-03, N-05, N-06 | | | | | | | | | N-01, N-04 |

Not in v1: C-09 (split view), P-03 (Windows).
