# JSON Visual Editor — Requirements Checklist (Draft v0.2)

> **วิธีใช้ไฟล์นี้ / How to use this file**
>
> - `- [ ]` = requirement ที่เสนอไว้ ถ้าเห็นด้วยให้ติ๊ก `- [x]`, ไม่เอาให้ลบบรรทัดหรือขีดฆ่า `~~...~~`
> - เพิ่มความเห็นได้ใต้ข้อนั้นด้วย `> NOTE: ...`
> - แต่ละข้อมีรหัส (เช่น `V-12`) เพื่อให้อ้างอิงกลับได้ง่าย ไม่ต้องเรียงใหม่ถ้าลบบางข้อ
> - ข้อที่ขึ้นต้นด้วย **(ref)** มาจากภาพ mockup / toolbar ซึ่งเป็นแค่ไอเดียอ้างอิง ไม่ได้ผูกมัด 100%
> - ข้อที่ขึ้นต้นด้วย **(proposed)** ไม่ได้อยู่ใน prompt.txt แต่ผมเสนอเพิ่มเพราะน่าจะจำเป็น
> - เหตุผลและการตัดสินใจทั้งหมดอยู่ใน `decision-log.md` (ไฟล์นี้เก็บเฉพาะ requirement)

**Product:** Flutter desktop app for editing JSON files. **macOS only** for now (Windows later).
**Core idea:** one document, two switchable modes, like a Markdown editor's _Code_ / _Preview_:

- **Code mode** — text-based editor with syntax highlighting.
- **Visual mode** — node-based tree editor.

---

## 1. Core: two modes on one document

- [x] **C-01** One open document can be shown in **Code mode** or **Visual mode**; user switches between them at any time.
- [x] **C-02** (proposed) Toggle via a toolbar button (segmented `Code | Visual`) and a keyboard shortcut (e.g. `Cmd/Ctrl+E`).
- [x] **C-03** Both modes edit the same underlying data; an edit in one mode is reflected when switching to the other.
- [x] **C-04** (proposed) If the text in Code mode is not valid JSON, switching to Visual mode is blocked and the error (line/column + message) is shown.
- [x] **C-05** (proposed) Key order is preserved exactly as in the source file in both modes.
- [x] **C-06** (proposed) Undo / redo history works across mode switches.
- [x] **C-07** (proposed) Selection / scroll position is kept in sync when switching (e.g. the node at the cursor in Code mode is selected in Visual mode).
- [x] **C-08** (proposed) Remember the last-used mode **per file**.
- [ ] **C-09** (ref, **later, not v1**) The mockup shows both panes side by side: a split view (Code + Visual together) as a third layout.
- [x] **C-10** Formatting: editing only in Code mode keeps the original whitespace/indentation; after an edit in Visual mode, the whole document is re-formatted (indent per S-07) when shown in Code mode.

## 2. Code mode (Editor side)

### 2.1 Display

- [x] **E-01** Open a `.json` source file and display it with **syntax highlighting** (keys, strings, numbers, booleans, null, punctuation in distinct colors).
- [x] **E-02** **Collapsible hierarchy** (code folding) for objects `{}` and arrays `[]`, with fold arrows in the gutter.
- [x] **E-03** Line numbers in the gutter.
- [x] **E-04** Editor understands JSON structure: key-value pairs, arrays, and data types (string, number, boolean, null, object, array).
- [x] **E-05** (proposed) Live validation: underline syntax errors and show the message on hover / in a status bar.
- [x] **E-06** **Word wrap toggle**; when on, long lines wrap to the window width.
- [x] **E-07** (ref, guide only) Mockup color scheme for the code pane (dark purple): keys pink/magenta, strings green, numbers blue, booleans orange. **Actual colors follow the One Light / One Dark themes (S-01).**
- [x] **E-08** (proposed) Bracket matching highlight and auto-closing of `{ [ "`.
- [x] **E-09** (proposed) Format / Pretty-print (indent per S-07: 2 spaces / 4 spaces / tab) and Minify commands.

### 2.2 Editing helpers

- [x] **E-10** **Auto-complete keys** that already exist in the document while typing a key.
- [x] **E-11** (proposed) Suggestions ranked by relevance: keys used in sibling objects at the same path first, then all other keys in the file.
- [x] **E-12** (proposed) Find / Replace (`Cmd/Ctrl+F`, `Cmd/Ctrl+H`), optionally with regex.

### 2.3 Copy / paste

- [x] **E-13** Code mode uses normal text copy / cut / paste. Node operations (drag & drop, node copy/paste) are Visual mode only, see 3.7.

## 3. Visual mode (node-based)

### 3.1 Tree layout (from mockup)

- [x] **V-01** Show the JSON as a **node-based tree**: each key/element is one row ("node"); children are indented under their parent.
- [x] **V-02** (ref) Vertical / L-shaped **guide lines** connect a parent to its children (as in the mockup).
- [x] **V-03** (ref) Each row is a light rounded "card" with a subtle background, spaced with a small gap between rows.
- [x] **V-04** Node row layout:
      `[+/-] [type icon] key : value ———————— (# children) [delete ×]`
  - `+/-` expand/collapse toggle (only for object/array nodes).
  - type icon = data-type badge (see V-10).
  - `key : value` on the left side.
  - `————` = flexible space so the left group is left-aligned and the right group is right-aligned.
  - `(# children)` count on the right — (ref) mockup shows `{ 2 }`, `{ 5 }` for objects; (proposed) `[ n ]` for arrays.
  - `×` delete icon at the far right of every row.
- [x] **V-05** (ref) Container rows (object/array) show a small grey chevron box (`>` collapsed, `v` expanded) **before** their type icon (Object / Array badge), then the key and child count. Every row has a type icon (V-04), so V-11 works on containers too.
- [x] **V-06** (ref) Leaf rows show: type badge, key (colored with the type color), and the value (grey) aligned in a second column so values line up vertically among siblings.
- [x] **V-07** Array items show their index (`0`, `1`, `2` …) in place of a key (proposed).
- [x] **V-08** (ref) Mockup shows a small resize handle at the bottom-right of the panel.

### 3.2 Data types, icons and colors (from mockup)

- [x] **V-10** Each type has its own icon + color:
      | Type | Icon (ref) | Color (ref) |
      |---|---|---|
      | Object | cube outline | grey/neutral |
      | String | `S` badge | green |
      | Number | `12` badge | blue |
      | Array | `[ ]` badge | purple |
      | Boolean | power-button icon | orange |
      | Null | `∅` badge | grey (supported; not in mockup) |
- [x] **V-11** **Single-click the type icon** → dropdown menu to change the data type.
- [x] **V-12** Dropdown lists: Object, Array, (divider), String, Number, Boolean, Null — each with its icon and color.
- [x] **V-13** Changing type keeps the value if it can be converted (e.g. `"123"` → `123`); otherwise it **resets to the default** (`{}`, `[]`, `""`, `0`, `false`, `null`). Undoable.

### 3.3 Expand / collapse

- [x] **V-20** Expand/collapse a single node with its `+/-` toggle.
- [x] **V-21** **Show/hide collapsible nodes on multiple levels at once**: Expand all, Collapse all, (proposed) Expand to level N (1, 2, 3 …).
- [x] **V-22** (proposed) Alt/Option-click on a toggle expands/collapses that node and all its descendants.

### 3.4 Viewing and editing values

- [x] **V-30** Default value display follows the S-04 toggle: single-line (truncated) or multi-line up to **10 lines** with a scrollbar. **Single-click a node** switches that node only between the two.
- [x] **V-31** **Double-click the key** → inline rename.
- [x] **V-32** **Double-click the value** → inline edit.
- [x] **V-33** (proposed) `Enter` confirms, `Esc` cancels inline editing; invalid input for the type (e.g. text in a Number) is rejected with a message.
- [x] **V-34** Node content **wraps to the window width** when word wrap is on (see E-06 / S-05).
- [x] **V-35** Delete a node with the `×` icon (proposed: also `Delete/Backspace` key; undoable, no confirm dialog).

### 3.5 Adding nodes (from mockup)

- [x] **V-40** (ref) Each expanded object/array ends with a **`+ ADD`** row at the bottom of its children, aligned with the guide line.
- [x] **V-41** (ref) Clicking `+ ADD` shows an **inline add form** in place of the row:
      `[ Name ] [ type dropdown (default: String) ] [ Value ] [✓ confirm (blue)] [× cancel]`
- [x] **V-42** The type dropdown in the add form is the same menu as V-12.
- [x] **V-43** (proposed) When type is Object, Array or Null, the Value field is hidden/disabled (empty container or `null` is created).
- [x] **V-44** (proposed) In an array, the Name field is hidden (items have no key).
- [x] **V-45** (proposed) The Name field offers key auto-complete (same as E-10).
- [x] **V-46** Duplicate keys (add, paste, rename, drag): show a **warning** but allow it. Files that already contain duplicate keys open and save without losing any entry (N-02).

### 3.6 Rich value preview

- [x] **V-50** Detect **base64-encoded images** in string values and show a **thumbnail** in the node row. Image **URLs** are not supported in v1 (later).
- [x] **V-51** Click the thumbnail → **popup** showing the full-size image (proposed: with size, MIME type, dimensions).
- [x] **V-52** Detection runs as an initial scan when the file opens (proposed: in the background, so big files open fast); supports `data:image/...;base64,` URIs and (proposed) raw base64 by checking image magic bytes (PNG, JPEG, GIF, WebP, SVG?).
- [x] **V-53** Show **HTML** and **Markdown** string values in a **popup viewer**.
- [x] **V-54** Popup opens in **rendered preview** by default; a toggle switches to **source edit** with syntax highlighting; edits are saved back to the JSON string (escaped correctly).
- [x] **V-55** HTML/Markdown detection: **both** auto-detect and manual "Open as HTML / Markdown / Plain text" in the context menu.

### 3.7 Node operations in Visual mode

- [x] **E-20** **Drag and drop** a single tree node to move it; an **insertion line** shows where it will land while dragging.
- [x] **E-21** Drop targets: before a node, after a node, or (proposed) inside an object/array as a child.
- [x] **E-22** **Copy / paste** nodes.
- [x] **E-23** Only **one node** can be selected, copied, cut, dragged or duplicated at a time (no multi-select).
- [x] **E-24** Paste can insert **before** or **after** the selected node (e.g. two separate commands / context menu items).
- [x] **E-25** (proposed) Copied nodes go to the system clipboard as JSON text so they can be pasted into other apps.
- [x] **E-26** Pasting a key that already exists in the target object shows a **warning** but is allowed (see V-46).

- [x] **V-60** E-20 to E-26 above apply to Visual mode only.
- [x] **V-61** (proposed) Right-click context menu on a node: Copy, Cut, Paste before, Paste after, Duplicate, Delete, Add child, Change type, Copy path (e.g. `merge_config.rule_1.type`).
- [x] **V-62** (proposed) Keyboard navigation: arrow keys move selection, `←/→` collapse/expand.

## 4. General UI & toolbar

- [x] **G-01** Overall design follows **Material 3** conventions (components, spacing, elevation, color roles) except where this document says otherwise.
- [x] **G-02** (ref) Toolbar like the screenshot: rounded pill-shaped icon buttons in one row at the top of the window, integrated with the macOS title bar (traffic lights on the left).
- [x] **G-03** (ref) Toolbar buttons, left → right (to be adapted for JSON):
  - [x] Star — add / remove the current file to **favorites**
  - [x] Hamburger — show / hide the **sidebar** listing favorites and recent files
  - [x] `+` with dropdown `v` — New file / new from template (split button)
  - [x] Folder — Open file
  - [x] divider
  - [x] Search — Find in document
  - [x] (proposed) `Code | Visual` mode switch
  - [x] right side: Contrast icon — quick toggle light/dark theme
  - [x] right side: **Gear icon — Settings** (top-right, required by prompt)
- [x] **G-04** (ref) Document title / file name shown under the toolbar (as in screenshot).
- [x] **G-05** (ref) Native menu bar on macOS: File, Edit, View, Window, Help.
- [x] **G-06** (proposed) File operations: New, Open, Save, Save As, Close, Recent files; unsaved-changes indicator (`•`) and "Save changes?" prompt on close.
- [x] **G-07** (proposed) Drag a `.json` file onto the window to open it.
- [x] **G-08** (proposed) Status bar: validation status, cursor line/column (Code mode), selected node path (Visual mode), file encoding.
- [x] **G-09** One window per file. Opening another file opens a **new window** of the same app; opening a file that is already open brings its window to the front.

## 5. Settings (gear icon)

- [x] **S-01** **Theme**: two built-in themes similar to **One Light** and **One Dark**. (proposed) plus "Follow system".
- [x] **S-02** **Monospace font** for Code mode: default **IBM Plex Mono**, changeable to any monospace font installed on the system.
- [x] **S-03** **Default fonts** (bundled with the app):
  - UI text: **IBM Plex Sans**
  - Keys and data-type labels: **IBM Plex Mono**
  - Values: **IBM Plex Sans Thai Looped**
- [x] **S-04** Toggle: show node values as **multi-line** or **single-line** by default (Visual mode).
- [x] **S-05** (proposed) Word wrap on/off default.
- [x] **S-06** (proposed) Font size / zoom (`Cmd/Ctrl +/-`).
- [x] **S-07** (proposed) Indentation for saving: 2 spaces / 4 spaces / tab.
- [x] **S-08** Language: **Thai / English** (see section 7).
- [x] **S-09** (proposed) Settings are saved and restored on next launch.

## 6. Platform integration

- [x] **P-01** Runs on **macOS** only (Flutter desktop).
- [x] **P-02** **macOS**: register as an editor for `.json` (`CFBundleDocumentTypes` / UTI `public.json` in Info.plist) so the user can set it as the **default app** ("Open With → Change All").
- [ ] **P-03** (**later, not v1**) **Windows**: register a file association for `.json` (ProgID + "Open with" entry via the installer, e.g. MSIX or Inno Setup) so it can be chosen as the **default app**.
- [x] **P-04** Opening a file from Finder (double-click, Open With, command-line argument) opens it in a **new window** of the running app (see G-09).
- [x] **P-05** Packaging: local build for own use for now; **no signing / notarization** yet (needs an Apple Developer account to distribute).
- [x] **P-06** macOS keyboard shortcuts (`Cmd`).

## 7. Internationalization (i18n)

- [x] **I-01** UI supports **Thai** and **English**.
- [x] **I-02** (proposed) Default language follows the OS; user can override in Settings; change applies immediately without restart.
- [x] **I-03** Thai text in keys/values renders correctly (IBM Plex Sans Thai Looped), including vowel/tone marks and line breaking when wrapping.
- [x] **I-04** (proposed) Use Flutter's standard localization (`flutter_localizations` + ARB files) so new languages can be added later.

## 8. Non-functional (proposed)

- [x] **N-01** Opens and stays responsive with files up to **50 MB**.
- [x] **N-02** Never silently changes data: what is saved is valid JSON and keeps values exactly (e.g. big numbers / decimals not rounded).
- [x] **N-03** Encoding: read/write UTF-8 (handle BOM).
- [x] **N-04** Basic accessibility: keyboard usable, sufficient contrast in both themes.
- [x] **N-05** Unit + widget tests for the JSON model, mode sync, and node operations.
- [x] **N-06** Strict JSON only (no JSONC, JSON5 or `.jsonl`).
