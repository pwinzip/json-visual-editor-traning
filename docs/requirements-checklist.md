# JSON Visual Editor — Requirements Checklist (Draft v0.1)

> **วิธีใช้ไฟล์นี้ / How to use this file**
> - `- [ ]` = requirement ที่เสนอไว้ ถ้าเห็นด้วยให้ติ๊ก `- [x]`, ไม่เอาให้ลบบรรทัดหรือขีดฆ่า `~~...~~`
> - เพิ่มความเห็นได้ใต้ข้อนั้นด้วย `> NOTE: ...`
> - แต่ละข้อมีรหัส (เช่น `V-12`) เพื่อให้อ้างอิงกลับได้ง่าย ไม่ต้องเรียงใหม่ถ้าลบบางข้อ
> - ข้อที่ขึ้นต้นด้วย **(ref)** มาจากภาพ mockup / toolbar ซึ่งเป็นแค่ไอเดียอ้างอิง ไม่ได้ผูกมัด 100%
> - ข้อที่ขึ้นต้นด้วย **(proposed)** ไม่ได้อยู่ใน prompt.txt แต่ผมเสนอเพิ่มเพราะน่าจะจำเป็น
> - คำถามที่ยังไม่ชัดอยู่ในหัวข้อ **9. Open questions** ท้ายไฟล์

**Product:** Flutter desktop app (macOS + Windows) for editing JSON files.
**Core idea:** one document, two switchable modes, like a Markdown editor's *Code* / *Preview*:
- **Code mode** — text-based editor with syntax highlighting.
- **Visual mode** — node-based tree editor.

---

## 1. Core: two modes on one document

- [ ] **C-01** One open document can be shown in **Code mode** or **Visual mode**; user switches between them at any time.
- [ ] **C-02** (proposed) Toggle via a toolbar button (segmented `Code | Visual`) and a keyboard shortcut (e.g. `Cmd/Ctrl+E`).
- [ ] **C-03** Both modes edit the same underlying data; an edit in one mode is reflected when switching to the other.
- [ ] **C-04** (proposed) If the text in Code mode is not valid JSON, switching to Visual mode is blocked and the error (line/column + message) is shown.
- [ ] **C-05** (proposed) Key order is preserved exactly as in the source file in both modes.
- [ ] **C-06** (proposed) Undo / redo history works across mode switches.
- [ ] **C-07** (proposed) Selection / scroll position is kept in sync when switching (e.g. the node at the cursor in Code mode is selected in Visual mode).
- [ ] **C-08** (proposed) Remember the last-used mode (globally or per file).
- [ ] **C-09** (ref) The mockup shows both panes side by side. Optional: a split view (Code + Visual together) as a third layout.

## 2. Code mode (Editor side)

### 2.1 Display
- [ ] **E-01** Open a `.json` source file and display it with **syntax highlighting** (keys, strings, numbers, booleans, null, punctuation in distinct colors).
- [ ] **E-02** **Collapsible hierarchy** (code folding) for objects `{}` and arrays `[]`, with fold arrows in the gutter.
- [ ] **E-03** Line numbers in the gutter.
- [ ] **E-04** Editor understands JSON structure: key-value pairs, arrays, and data types (string, number, boolean, null, object, array).
- [ ] **E-05** (proposed) Live validation: underline syntax errors and show the message on hover / in a status bar.
- [ ] **E-06** **Word wrap toggle**; when on, long lines wrap to the window width.
- [ ] **E-07** (ref) Mockup color scheme for the code pane (dark): keys pink/magenta, string values green, numbers blue, booleans orange, punctuation dimmed, line numbers dimmed.
- [ ] **E-08** (proposed) Bracket matching highlight and auto-closing of `{ [ "`.
- [ ] **E-09** (proposed) Format / Pretty-print (indent 2 or 4 spaces) and Minify commands.

### 2.2 Editing helpers
- [ ] **E-10** **Auto-complete keys** that already exist in the document while typing a key.
- [ ] **E-11** (proposed) Suggestions ranked by relevance: keys used in sibling objects at the same path first, then all other keys in the file.
- [ ] **E-12** (proposed) Find / Replace (`Cmd/Ctrl+F`, `Cmd/Ctrl+H`), optionally with regex.

### 2.3 Node operations (see Open question Q-01 — these may belong to Visual mode)
- [ ] **E-20** **Drag and drop** tree node(s) to move them; an **insertion line** shows where they will land while dragging.
- [ ] **E-21** Drop targets: before a node, after a node, or (proposed) inside an object/array as a child.
- [ ] **E-22** **Copy / paste** nodes.
- [ ] **E-23** Copying multiple sibling nodes at once is **prevented** (see Q-02 — please confirm the intended rule).
- [ ] **E-24** Paste can insert **before** or **after** the selected node (e.g. two separate commands / context menu items).
- [ ] **E-25** (proposed) Copied nodes go to the system clipboard as JSON text so they can be pasted into other apps.
- [ ] **E-26** (proposed) When pasting into an object where the key already exists, auto-rename (`key_copy`, `key_2` …) or ask the user (see Q-05).

## 3. Visual mode (node-based)

### 3.1 Tree layout (from mockup)
- [ ] **V-01** Show the JSON as a **node-based tree**: each key/element is one row ("node"); children are indented under their parent.
- [ ] **V-02** (ref) Vertical / L-shaped **guide lines** connect a parent to its children (as in the mockup).
- [ ] **V-03** (ref) Each row is a light rounded "card" with a subtle background, spaced with a small gap between rows.
- [ ] **V-04** Node row layout:
      `[+/-] [type icon] key : value ———————— (# children) [delete ×]`
  - `+/-` expand/collapse toggle (only for object/array nodes).
  - type icon = data-type badge (see V-10).
  - `key : value` on the left side.
  - `————` = flexible space so the left group is left-aligned and the right group is right-aligned.
  - `(# children)` count on the right — (ref) mockup shows `{ 2 }`, `{ 5 }` for objects; (proposed) `[ n ]` for arrays.
  - `×` delete icon at the far right of every row.
- [ ] **V-05** (ref) Container rows (object/array) show a small grey chevron box (`>` collapsed, `v` expanded) instead of a type badge, plus the key and child count.
- [ ] **V-06** (ref) Leaf rows show: type badge, key (colored with the type color), and the value (grey) aligned in a second column so values line up vertically among siblings.
- [ ] **V-07** Array items show their index (`0`, `1`, `2` …) in place of a key (proposed).
- [ ] **V-08** (ref) Mockup shows a small resize handle at the bottom-right of the panel.

### 3.2 Data types, icons and colors (from mockup)
- [ ] **V-10** Each type has its own icon + color:
  | Type | Icon (ref) | Color (ref) |
  |---|---|---|
  | Object | cube outline | grey/neutral |
  | String | `S` badge | green |
  | Number | `12` badge | blue |
  | Array | `[ ]` badge | purple |
  | Boolean | power-button icon | orange |
  | Null | (proposed) `∅` badge | grey — **not in mockup**, see Q-03 |
- [ ] **V-11** **Single-click the type icon** → dropdown menu to change the data type.
- [ ] **V-12** (ref) Dropdown lists: Object, (divider), String, Number, Array, Boolean — each with its icon and color.
- [ ] **V-13** (proposed) Converting type tries to keep the value (e.g. `"123"` → `123`); if not possible, use a default value (`""`, `0`, `false`, `{}`, `[]`, `null`) and allow undo (see Q-04).

### 3.3 Expand / collapse
- [ ] **V-20** Expand/collapse a single node with its `+/-` toggle.
- [ ] **V-21** **Show/hide collapsible nodes on multiple levels at once**: Expand all, Collapse all, (proposed) Expand to level N (1, 2, 3 …).
- [ ] **V-22** (proposed) Alt/Option-click on a toggle expands/collapses that node and all its descendants.

### 3.4 Viewing and editing values
- [ ] **V-30** **Single-click a node** → its value switches from single-line (truncated) to multi-line, showing up to **10 lines** with a scrollbar for the rest.
- [ ] **V-31** **Double-click the key** → inline rename.
- [ ] **V-32** **Double-click the value** → inline edit.
- [ ] **V-33** (proposed) `Enter` confirms, `Esc` cancels inline editing; invalid input for the type (e.g. text in a Number) is rejected with a message.
- [ ] **V-34** Node content **wraps to the window width** when word wrap is on (see E-06 / S-05).
- [ ] **V-35** Delete a node with the `×` icon (proposed: also `Delete/Backspace` key; undoable, no confirm dialog).

### 3.5 Adding nodes (from mockup)
- [ ] **V-40** (ref) Each expanded object/array ends with a **`+ ADD`** row at the bottom of its children, aligned with the guide line.
- [ ] **V-41** (ref) Clicking `+ ADD` shows an **inline add form** in place of the row:
      `[ Name ] [ type dropdown (default: String) ] [ Value ] [✓ confirm (blue)] [× cancel]`
- [ ] **V-42** The type dropdown in the add form is the same menu as V-12.
- [ ] **V-43** (proposed) When type is Object or Array, the Value field is hidden/disabled and an empty container is created.
- [ ] **V-44** (proposed) In an array, the Name field is hidden (items have no key).
- [ ] **V-45** (proposed) The Name field offers key auto-complete (same as E-10).
- [ ] **V-46** (proposed) Duplicate key in the same object is warned before confirming (see Q-05).

### 3.6 Rich value preview
- [ ] **V-50** Detect **base64-encoded images** in string values and show a **thumbnail** in the node row.
- [ ] **V-51** Click the thumbnail → **popup** showing the full-size image (proposed: with size, MIME type, dimensions).
- [ ] **V-52** Detection runs as an initial scan when the file opens (proposed: in the background, so big files open fast); supports `data:image/...;base64,` URIs and (proposed) raw base64 by checking image magic bytes (PNG, JPEG, GIF, WebP, SVG?).
- [ ] **V-53** Show **HTML** and **Markdown** string values in a **popup viewer**.
- [ ] **V-54** Popup opens in **rendered preview** by default; a toggle switches to **source edit** with syntax highlighting; edits are saved back to the JSON string (escaped correctly).
- [ ] **V-55** (proposed) How the app knows a string is HTML/Markdown: auto-detect + manual "Open as HTML / Markdown / Plain text" in a context menu (see Q-06).

### 3.7 Node operations in Visual mode
- [ ] **V-60** Drag and drop, copy/paste work in Visual mode as described in E-20 – E-26 (see Q-01).
- [ ] **V-61** (proposed) Right-click context menu on a node: Copy, Cut, Paste before, Paste after, Duplicate, Delete, Add child, Change type, Copy path (e.g. `merge_config.rule_1.type`).
- [ ] **V-62** (proposed) Keyboard navigation: arrow keys move selection, `←/→` collapse/expand.

## 4. General UI & toolbar

- [ ] **G-01** Overall design follows **Material 3** conventions (components, spacing, elevation, color roles) except where this document says otherwise.
- [ ] **G-02** (ref) Toolbar like the screenshot: rounded pill-shaped icon buttons in one row at the top of the window, integrated with the macOS title bar (traffic lights on the left).
- [ ] **G-03** (ref) Toolbar buttons, left → right (to be adapted for JSON):
  - [ ] Star — favorites / bookmarked files (see Q-08)
  - [ ] Hamburger — toggle sidebar (recent files / outline) (see Q-08)
  - [ ] `+` with dropdown `v` — New file / new from template (split button)
  - [ ] Folder — Open file
  - [ ] divider
  - [ ] Search — Find in document
  - [ ] (proposed) `Code | Visual` mode switch
  - [ ] right side: Contrast icon — quick toggle light/dark theme
  - [ ] right side: **Gear icon — Settings** (top-right, required by prompt)
- [ ] **G-04** (ref) Document title / file name shown under the toolbar (as in screenshot).
- [ ] **G-05** (ref) Native menu bar on macOS: File, Edit, View, Window, Help.
- [ ] **G-06** (proposed) File operations: New, Open, Save, Save As, Close, Recent files; unsaved-changes indicator (`•`) and "Save changes?" prompt on close.
- [ ] **G-07** (proposed) Drag a `.json` file onto the window to open it.
- [ ] **G-08** (proposed) Status bar: validation status, cursor line/column (Code mode), selected node path (Visual mode), file encoding.
- [ ] **G-09** (proposed) One window per file, or tabs (see Q-09).

## 5. Settings (gear icon)

- [ ] **S-01** **Theme**: two built-in themes similar to **One Light** and **One Dark**. (proposed) plus "Follow system".
- [ ] **S-02** **Monospace font** for Code mode: choose from fonts installed on the system.
- [ ] **S-03** **Default fonts** (bundled with the app):
  - UI text: **IBM Plex Sans**
  - Keys and data-type labels: **IBM Plex Mono**
  - Values: **IBM Plex Sans Thai Looped**
- [ ] **S-04** Toggle: show node values as **multi-line** or **single-line** by default (Visual mode).
- [ ] **S-05** (proposed) Word wrap on/off default.
- [ ] **S-06** (proposed) Font size / zoom (`Cmd/Ctrl +/-`).
- [ ] **S-07** (proposed) Indentation for saving: 2 spaces / 4 spaces / tab.
- [ ] **S-08** Language: **Thai / English** (see section 7).
- [ ] **S-09** (proposed) Settings are saved and restored on next launch.

## 6. Platform integration

- [ ] **P-01** Runs on **macOS** and **Windows** (Flutter desktop).
- [ ] **P-02** **macOS**: register as an editor for `.json` (`CFBundleDocumentTypes` / UTI `public.json` in Info.plist) so the user can set it as the **default app** ("Open With → Change All").
- [ ] **P-03** **Windows**: register a file association for `.json` (ProgID + "Open with" entry via the installer, e.g. MSIX or Inno Setup) so it can be chosen as the **default app**.
- [ ] **P-04** Opening a file from Finder/Explorer (double-click, Open With, command-line argument) opens it in the app; if the app is already running, open it in the existing instance (proposed).
- [ ] **P-05** (proposed) Installer / packaging: `.dmg` (macOS, signed + notarized?) and `.msix` or `.exe` installer (Windows) — see Q-10.
- [ ] **P-06** (proposed) Platform-appropriate shortcuts (`Cmd` on macOS, `Ctrl` on Windows).

## 7. Internationalization (i18n)

- [ ] **I-01** UI supports **Thai** and **English**.
- [ ] **I-02** (proposed) Default language follows the OS; user can override in Settings; change applies immediately without restart.
- [ ] **I-03** Thai text in keys/values renders correctly (IBM Plex Sans Thai Looped), including vowel/tone marks and line breaking when wrapping.
- [ ] **I-04** (proposed) Use Flutter's standard localization (`flutter_localizations` + ARB files) so new languages can be added later.

## 8. Non-functional (proposed)

- [ ] **N-01** Opens and stays responsive with files up to __ MB (please fill in a target, see Q-07).
- [ ] **N-02** Never silently changes data: what is saved is valid JSON and keeps values exactly (e.g. big numbers / decimals not rounded).
- [ ] **N-03** Encoding: read/write UTF-8 (handle BOM).
- [ ] **N-04** Basic accessibility: keyboard usable, sufficient contrast in both themes.
- [ ] **N-05** Unit + widget tests for the JSON model, mode sync, and node operations.

---

## 9. Open questions (คำถามที่ยังไม่ชัด — ช่วยตอบใต้แต่ละข้อได้เลย)

- **Q-01** prompt.txt puts *drag & drop tree nodes* and *copy/paste nodes* under **Editor side**. Should these work in **Code mode** (text editor), **Visual mode**, or both? (My guess: mainly Visual mode; Code mode uses normal text copy/paste.)
  > ANSWER:
- **Q-02** "Copy multiple sibling nodes is prevented" — do you mean (a) only one node can be copied at a time, or (b) multiple nodes can be copied **only if they are siblings** (non-siblings prevented)?
  > ANSWER:
- **Q-03** The mockup type menu has no **Null** type. Should `null` be supported as a type in the dropdown?
  > ANSWER:
- **Q-04** Changing a type when the value can't be converted (e.g. Object with children → String): convert to JSON text, reset to default, or ask first?
  > ANSWER:
- **Q-05** **Duplicate keys** — the mockup itself contains `join_config` several times in one object (allowed by the JSON spec but most parsers keep only the last). Keep and show duplicates, warn, or block?
  > ANSWER:
- **Q-06** HTML/Markdown detection: automatic, manual "Open as…", or both? Should the image thumbnail also support image **URLs** (not just base64)?
  > ANSWER:
- **Q-07** Expected largest file size (e.g. 1 MB, 10 MB, 100 MB)? This affects the architecture (lazy rendering, background parsing).
  > ANSWER:
- **Q-08** Toolbar's **Star** and **Hamburger** buttons — what should they do in this app (favorites? sidebar outline? recent files?), or drop them?
  > ANSWER:
- **Q-09** One window per file, or tabs for multiple files?
  > ANSWER:
- **Q-10** Distribution: do you need code signing / notarization (macOS) and a Windows installer, or just build locally for now? Linux needed?
  > ANSWER:
- **Q-11** Does Code mode need to preserve the original formatting (whitespace, indentation) after editing in Visual mode, or is re-formatting on switch OK?
  > ANSWER:
- **Q-12** Support JSON variants (JSON with comments / JSONC, JSON5, `.jsonl`), or strict JSON only?
  > ANSWER:
- **Q-13** "Multi-line up to 10 lines" (V-30) vs the Settings toggle (S-04): when the toggle is set to multi-line, should every node show up to 10 lines without clicking?
  > ANSWER:
