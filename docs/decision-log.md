# Decision log — JSON Visual Editor

> Append-only. Add each new decision as a new entry at the **bottom**; do not edit or delete old entries. To change a decision, add a new entry that says which one it replaces.
> Requirements themselves live in `requirements-checklist.md`; this file records **why** they are the way they are.
>
> Entry format:
> `### D-NNN — <title>` / **Date** / **Decision** / **Affects** (requirement IDs) / **Source**

---

### D-001 — Drag & drop and node copy/paste are Visual mode only
- **Date:** 2026-11-22
- **Decision:** Node operations (drag & drop, node copy/paste) belong to Visual mode. Code mode uses normal text copy/paste.
- **Affects:** E-13, E-20 – E-26, V-60
- **Source:** Q-01 answered by ping

### D-002 — Only one node at a time
- **Date:** 2026-11-22
- **Decision:** Only one node can be selected, copied, cut, dragged or duplicated at a time (no multi-select).
- **Affects:** E-20, E-23
- **Source:** Q-02 answered (a) by ping; review R-03

### D-003 — Null is a supported type
- **Date:** 2026-11-22
- **Decision:** `null` is a type in the dropdown (Object, Array, divider, String, Number, Boolean, Null). No Value field for Null.
- **Affects:** V-10, V-12, V-43
- **Source:** Q-03 answered by ping; review R-04

### D-004 — Type change: convert if possible, otherwise reset to default
- **Date:** 2026-11-22
- **Decision:** Keep the value if it converts (e.g. `"123"` → `123`); otherwise reset to the type's default value. Undoable.
- **Affects:** V-13
- **Source:** Q-04 answered "reset to default" by ping; review R-11

### D-005 — Duplicate keys: warn but allow
- **Date:** 2026-11-22
- **Decision:** Any action that creates a duplicate key (add, paste, rename, drag) shows a warning but is allowed. Files that already have duplicate keys open and save without data loss.
- **Affects:** E-26, V-46, N-02
- **Source:** Q-05 answered "warn" by ping; review R-12

### D-006 — HTML/Markdown detection auto + manual; images base64 only in v1
- **Date:** 2026-11-22
- **Decision:** HTML/Markdown detected automatically and also via manual "Open as…". Image thumbnails support base64 only in v1; image URLs later.
- **Affects:** V-50, V-55
- **Source:** Q-06 answered "both" by ping; review R-10

### D-007 — File size target 50 MB
- **Date:** 2026-11-22
- **Decision:** Must open and stay responsive with files up to 50 MB.
- **Affects:** N-01
- **Source:** Q-07 answered by ping; review R-09

### D-008 — Star = favorites, Hamburger = sidebar
- **Date:** 2026-11-22
- **Decision:** Star adds/removes the current file to favorites. Hamburger shows/hides a sidebar listing favorites and recent files.
- **Affects:** G-03
- **Source:** Q-08 answered "favorites sidebar" by ping; review R-13

### D-009 — One window per file
- **Date:** 2026-11-22
- **Decision:** One window per file. Opening another file opens a new window of the same app; opening an already-open file brings its window to the front.
- **Affects:** G-09, P-04
- **Source:** Q-09 answered "one window per file, but support multiple instances" by ping; review R-05

### D-010 — macOS only; no signing for now
- **Date:** 2026-11-22
- **Decision:** v1 targets macOS only (Windows later). Local build for own use; no signing/notarization yet.
- **Affects:** Product line, P-01, P-03, P-04, P-05, P-06
- **Source:** Q-10 answered "macOS only" by ping; review R-01, R-14

### D-011 — Re-format after Visual edits
- **Date:** 2026-11-22
- **Decision:** Editing only in Code mode keeps original formatting. After an edit in Visual mode, the whole document is re-formatted (indent per S-07).
- **Affects:** C-10
- **Source:** Q-11 answered "do it after editing in visual mode" by ping; review R-15

### D-012 — Strict JSON only
- **Date:** 2026-11-22
- **Decision:** Only strict JSON. No JSONC, JSON5 or `.jsonl`.
- **Affects:** N-06
- **Source:** Q-12 answered by ping

### D-013 — Settings toggle controls multi-line display
- **Date:** 2026-11-22
- **Decision:** The S-04 toggle sets the default (single-line or up to 10 lines + scrollbar). Single-click toggles one node only.
- **Affects:** S-04, V-30
- **Source:** Q-13 answered "use toggle" by ping; review R-06

### D-014 — Every node row has a type icon
- **Date:** 2026-11-22
- **Decision:** Object/array rows show a chevron **before** their type icon, so type change works on containers too.
- **Affects:** V-04, V-05, V-11
- **Source:** review R-07, approved by ping

### D-015 — Colors follow One Light / One Dark
- **Date:** 2026-11-22
- **Decision:** Syntax and UI colors follow the One Light / One Dark themes; the mockup's purple palette is a guide only.
- **Affects:** E-07, S-01
- **Source:** review R-08, approved by ping

### D-016 — Smaller choices
- **Date:** 2026-11-22
- **Decision:** Last mode remembered per file (C-08). Split view is later, not v1 (C-09). Indent options 2 / 4 spaces / tab (E-09, S-07). Code mode font defaults to IBM Plex Mono, changeable (S-02).
- **Affects:** C-08, C-09, E-09, S-02, S-07
- **Source:** review R-16 – R-19, approved by ping

### D-017 — Working rules
- **Date:** 2026-11-22
- **Decision:** (1) GitHub feature-branch workflow: each piece of work on its own branch, merged through a pull request. (2) Every reply uses the three message types in `AGENTS.md` (`[DECISION]`, `[INFORM]`, no label for logs). (3) Work one phase at a time, starting with the first phase only. (4) Compile and test locally; do not use GitHub Actions unless truly needed. (5) Tickets are created as GitHub issues with the `to-tickets` skill.
- **Affects:** process (all phases)
- **Source:** ping, project thread
