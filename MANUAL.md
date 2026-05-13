# Jira Ticket Planner — Manual

A single-file, no-build, browser-based planner for breaking Jira tickets into hour-sized blocks and scheduling them across two weeks.

---

## Quick start

1. Open `planner.html` in your browser (double-click it from Explorer, or drag it to a tab).
2. Best in **Chrome** or **Edge** if you want the **Link File** auto-save feature. Firefox works for everything else.
3. On first run, 5 demo projects seed into the backlog so you have something to drag around.

---

## Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ Planner  [Today] [←] range [→]  [+Block] [Import] [Export] [Link] │
├──────────────────────────────────────────────┬───────────────────┤
│ Week 1                                       │ BACKLOG | SEARCH  │
│ ┌─────┬─────┬─────┬─────┬─────┬───┐          │ ┌───────────────┐ │
│ │ Mon │ Tue │ Wed │ Thu │ Fri │S/S│          │ │  block        │ │
│ └─────┴─────┴─────┴─────┴─────┴───┘          │ │  block        │ │
│ Week 2                                       │ │  block        │ │
│ ┌─────┬─────┬─────┬─────┬─────┬───┐          │ └───────────────┘ │
│ │ Mon │ Tue │ Wed │ Thu │ Fri │S/S│          │                   │
│ └─────┴─────┴─────┴─────┴─────┴───┘          │                   │
└──────────────────────────────────────────────┴───────────────────┘
```

- **Two weeks** visible at once. Mon–Fri get full-height columns; **Sat/Sun stack** into the narrower 6th column.
- **Right pane** has two tabs:
  - **Backlog** — unscheduled blocks (no date)
  - **Search** — instant filter across every block ever created

---

## Blocks

A block is one hour-budget chunk of work tied to a Jira ticket key.

### Anatomy

```
┌────────────────────────────────┐
│ JIRA-123  ticket name…         │ ← colored header (project color)
├────────────────────────────────┤
│ 4h — short description         │ ← body
└────────────────────────────────┘
```

| Field         | Purpose                                        | Inline-editable? |
| ------------- | ---------------------------------------------- | ---------------- |
| **Jira key**  | Project + ticket number. Drag handle.          | Modal only       |
| **Name**      | Ticket name. Truncated with `…` when long.     | Yes              |
| **Hours**     | Block size: `8h`, `1.5h`, `1d 4h`              | Yes              |
| **Note**      | ~5 word description (max 60 chars)             | Yes              |
| **Done**      | Strikethrough + desaturate when checked        | Modal or right-click |

### Working with blocks

| Action               | How                                                                                                  |
| -------------------- | ---------------------------------------------------------------------------------------------------- |
| **Move**             | Drag the **Jira key** (the mono uppercase text on the colored band) onto a day or back to backlog.   |
| **Edit inline**      | Click any name / hours / note field. Saves on each keystroke.                                        |
| **Edit in modal**    | **Right-click → Edit…**, or **double-click** the block. Modal exposes every field including Jira key and Done. |
| **Clone**            | Right-click → **Clone**. Independent copy with `done: false`.                                        |
| **Mark done / undone** | Right-click → **Mark Done / Mark Undone**, or the checkbox in the modal.                             |
| **Delete**           | Right-click → **Delete**, or the Delete button bottom-left of the modal.                             |
| **Add blank block**  | Toolbar **+ Block**. Lands in backlog.                                                               |

### Sort order

Each block carries an `order` timestamp set on drop / add / clone / import. Days and the backlog sort by `order` ascending — so **the most recently moved block sinks to the bottom of its column**.

### Color coding

Each Jira key is hashed to a fixed slot in a 12-color palette derived from the brand colors:

```
peacock #194B6A   bondi #0080A3   plum #492049   crimson #AC1A34
+ 8 lifted/muted variants
```

Same key always gets the same color. Cloned blocks inherit the parent's color. With more than 12 distinct keys, colors repeat (deterministically).

### Done state

Done blocks **desaturate** and get a **strikethrough on the name**. Their hours still count toward the day total — finishing work doesn't shrink the day's tally.

### Stale (vanished from Jira)

Blocks whose Jira key was *not* in your most recent CSV import get a small **red dot** in the top-right corner.

---

## Day cells

| Element              | Meaning                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------- |
| **Day header**       | `WED  MAY 13` + total hours `4h/8h`                                                                  |
| **Soft 8h cap**      | Day total turns **red top-border + red hours text** when >8h. **Doesn't block** drops — you can over-schedule on purpose. |
| **Today**            | 2px accent-color border around the cell.                                                             |
| **Weekend (Sat/Sun)**| Slightly muted background, half-height of weekday cells.                                             |

Day hours total **every block on the day**, done or not. Marking a block done desaturates it but its hours stay in the day's total.

---

## Calendar navigation

| Action                 | How                                                  |
| ---------------------- | ---------------------------------------------------- |
| **Next 2 weeks**       | Toolbar **→**, or `Alt + →`                          |
| **Previous 2 weeks**   | Toolbar **←**, or `Alt + ←`                          |
| **Jump to today**      | Toolbar **Today**, or `Alt + T`                      |
| **Jump from search**   | In the Search tab, click any scheduled result's date pill. |

Pagination steps in **2-week chunks** (one full visible page at a time).

---

## Search tab

Right pane → **Search** tab. The input at the top instant-filters across **every block** (scheduled, backlog, done) by **Jira key** or **name** (case-insensitive substring).

- Results are **read-only display cards**. To edit one, right-click → Edit… or double-click.
- Each result shows a **location pill** at the bottom — either `IN BACKLOG` or the scheduled date (e.g. `MAY 12, 2026`).
- Click a date pill to jump the calendar to that week. The right pane also switches back to the **Backlog** tab so the date you jumped to is visible alongside the unscheduled blocks.
- **Esc** or the `×` button clears the query.

---

## Jira CSV import

Toolbar → **Import CSV**. Pick any Jira-exported CSV.

### Columns recognized

Auto-detected by header name (case-insensitive, exact match first, then substring fallback):

| Field        | Header names looked for                                              | Required? |
| ------------ | -------------------------------------------------------------------- | --------- |
| **Jira key** | `Issue key`, `Key`, `Issue ID`                                       | **Yes**   |
| **Name**     | `Summary`, `Title`, `Name`                                           | No (defaults blank) |
| **Hours**    | `Original Estimate`, `Remaining Estimate`, `Estimate`, `Time Estimate` | No (defaults to `8h`) |

### Hours parsing

Hours are parsed flexibly:

- `8h`, `1.5h` → hours directly
- `1d 4h` → 1 day = 8h, so 12h total
- Plain number → if > 1000, treated as **seconds** (Jira's default export format) and divided by 3600; otherwise treated as hours
- Unrecognized → `0h`. (CSV import substitutes `8h` as a fallback when the row's estimate is unparseable; the modal and inline edits leave it at `0h`.)

### Re-import behavior

Re-importing a fresh CSV:

- **New keys** → new blocks appear in the backlog
- **Existing keys** → kept exactly where they are (no duplicates; stale flag is cleared)
- **Keys missing from the new CSV** → flagged with a red corner dot

This means you can drop in your latest Jira export every morning and your existing schedule stays put.

---

## Persistence

Three layers, from automatic to manual:

### 1. localStorage (always on)

Every change writes to `localStorage` for the page's origin. Closing and reopening the tab restores state.

**Caveats:**

- `localStorage` is tied to the **exact file path**. Moving `planner.html` to a new folder gives you a fresh empty state at the new path.
- Clearing browser data wipes it.

### 2. Export / Restore (manual)

| Button       | Action                                                                                          |
| ------------ | ----------------------------------------------------------------------------------------------- |
| **Export**   | Downloads `planner-YYYY-MM-DD.json` to your browser's Downloads folder.                         |
| **Restore**  | Pick a backup JSON. **Wholesale replaces** current state (no merge). Asks for confirmation first. |

The "last exported: N days ago" nag appears in the bottom-right if it's been a while.

The exported JSON schema:

```json
{
  "blocks": [
    {
      "id": "b_abc123",
      "jiraKey": "AUTH-201",
      "name": "Authentication overhaul",
      "hours": 32,
      "note": "check token storage",
      "done": false,
      "date": "2026-05-12",
      "stale": false,
      "order": 1747000000000
    }
  ],
  "knownKeys": ["AUTH-201"],
  "weekOffset": 0,
  "theme": "light",
  "lastExport": 1747000000000,
  "seeded": true
}
```

### 3. Link File (Chrome / Edge only)

Toolbar → **Link File…**. A Save-As dialog opens. Pick `planner.json` anywhere (e.g. inside the repo folder). After that:

- Every change writes to that file, **debounced 1.5 seconds**.
- The file handle is persisted in **IndexedDB**, so reloads remember it.
- After a fresh browser session, Chrome may require you to click the **⚠ planner.json** button once to **re-grant** write permission. Auto-save resumes after.
- Click the linked button again to **change file** (type `change`) or **unlink** (type `unlink`).

Firefox doesn't support the File System Access API — the button stays disabled there.

#### Bootstrap order matters

If you want to start from the blank `planner.json` shipped in this repo:

1. **Restore** first — pick the blank `planner.json`. Calendar wipes.
2. **Link File** second — pick the same `planner.json`. Auto-save kicks in.

Doing it in the opposite order overwrites the blank file with whatever was previously in `localStorage`.

---

## Themes

Toolbar → **Dark / Light** toggle. Persists in `localStorage`.

Brand palette:

```
peacock  #194B6A   primary dark / text / borders
bondi    #0080A3   accent / today / hover / focus
plum     #492049   special / drag-active
ice      #DFE4F0   light backgrounds
tan      #F1E4D2   warm surfaces (backlog pane)
crimson  #AC1A34   alerts / overflow / errors / delete
```

---

## Keyboard shortcuts

| Keys                          | Action                                       |
| ----------------------------- | -------------------------------------------- |
| `Alt + →`                     | Forward 2 weeks                              |
| `Alt + ←`                     | Back 2 weeks                                 |
| `Alt + T`                     | Jump to today                                |
| `Esc`                         | Close modal / dismiss context menu / clear search |
| `Enter` (inside modal inputs) | Save the modal                               |
| Right-click on a block        | Context menu (Edit, Clone, Done, Delete)     |
| Double-click on a block       | Open the edit modal                          |
| Right-click in an input       | Native browser menu (paste, copy, etc.)      |

---

## File layout

```
jira-ticket-planner/
├── planner.html          ← the entire app (single file, no build)
├── planner.json          ← gitignored; your linked auto-save file
├── MANUAL.md             ← this document (the canonical reference)
├── Planner-Manual.pptx   ← slide-deck version of this manual
├── Planner-Manual.pdf    ← exported PDF version of the deck
├── _build_pptx.ps1       ← script that regenerates the .pptx
├── .gitignore
└── .git/
```

`planner.json` and any `planner-*.json` exports are gitignored, so your scheduling data never gets pushed. The slide deck and PDF mirror this manual visually — open whichever format you prefer.

---

## Browser compatibility

| Feature                          | Chrome / Edge        | Firefox                       |
| -------------------------------- | -------------------- | ----------------------------- |
| Calendar, blocks, drag-drop      | ✓                    | ✓                             |
| Inline editing, modal            | ✓                    | ✓                             |
| CSV import, JSON export/restore  | ✓                    | ✓                             |
| `localStorage` on `file://`      | ✓ (more eviction-prone) | ✓ (most reliable)             |
| **Link File** auto-save          | ✓                    | ✗ (button disabled)           |
| Light / dark theme               | ✓                    | ✓                             |

Use Chrome/Edge if you want auto-save-to-disk. Otherwise Firefox is the most reliable choice for plain `file://` local storage.

---

## Troubleshooting

| Symptom                              | Fix                                                                                          |
| ------------------------------------ | -------------------------------------------------------------------------------------------- |
| Calendar is empty after reopening    | You're opening from a different path. `localStorage` is per-path. Use the same location.     |
| Linked file button shows ⚠           | Chrome needs you to re-grant write permission. Click the button.                              |
| Demo projects keep reappearing       | The `seeded` flag isn't `true` in your state. Restore from a backup that has `"seeded": true`, or delete them once and they'll stay gone. |
| CSV import says "No Key column found"| The CSV's column header isn't `Issue key`, `Key`, or `Issue ID`. Rename it and retry.        |
| Hours come in as huge weird numbers  | The CSV's Estimate column is in seconds. The importer should auto-convert; edit affected blocks if a column looked ambiguous. |
| Block drags don't start              | You need to grab the **Jira key** specifically. Body inputs don't initiate drags.            |

---

## Glossary

- **Block** — One hour-budgeted unit of work, always tied to a Jira key.
- **Backlog** — Unscheduled blocks (no `date` field).
- **Scheduled** — Blocks with a `date` field set to `YYYY-MM-DD`.
- **Done** — Completed blocks. Visually muted; hours not counted toward day total.
- **Stale** — Block whose Jira key wasn't in the most recent CSV import.
- **Order** — Sort timestamp set on drop / add / clone / import. Lower = higher in the list.
- **Linked file** — A user-picked JSON file on disk that the page auto-writes to (Chrome/Edge only).
