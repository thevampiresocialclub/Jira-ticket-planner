# Jira Ticket Planner

A single-file, no-build, browser-based planner for breaking Jira tickets into hour-sized blocks and scheduling them across two weeks. Open the HTML, drag tickets onto days, and you're done.

![Project palette](https://img.shields.io/badge/Built_with-HTML_%2B_CSS_%2B_JS-194B6A?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-0080A3?style=flat-square)
![No build](https://img.shields.io/badge/Build-not_required-492049?style=flat-square)

---

## What it does

- **One HTML file**, no build step, no dependencies, no server. Drop it in any folder and open it in a browser.
- **Two-week view** — Mon–Fri get full-height columns, Sat/Sun stack into a narrower 6th column.
- **Drag-drop scheduling** — grab a ticket key, drop on a day.
- **Inline editing** for ticket name, hours, and a short note. Right-click or double-click any block to open a full editor modal.
- **Search tab** across every block you've ever created — by key or name, instant filter.
- **Jira CSV import** with column auto-detection (`Issue key`, `Summary`, `Original Estimate`, etc.).
- **Color-coded by project** — each Jira key hashes to a fixed slot in a 12-color palette.
- **Three persistence layers** — `localStorage` (always on), Export/Restore JSON, and Link File auto-save to disk (Chrome/Edge).
- **Light + dark themes** with a brand palette built for a long day at a work laptop.

---

## Quick start

1. Open `planner.html` in your browser. Chrome or Edge is best if you want the **Link File** auto-save feature. Firefox works for everything else.
2. On first run, 5 demo projects seed into the backlog so you have something to drag around.
3. Grab the **Jira key** (the bold mono text in the colored header band of any block) and drop it onto a day.
4. Click the **Name**, **Hours**, or **Note** fields to edit inline. Right-click or double-click for the full editor modal.
5. Paginate two weeks at a time with the arrows in the toolbar, or `Alt + ←` / `Alt + →`.

---

## Documentation

- **[MANUAL.md](MANUAL.md)** — the canonical, comprehensive user guide (layout, blocks, navigation, search, CSV import, persistence, keyboard shortcuts, troubleshooting).
- **[Planner-Manual.pdf](Planner-Manual.pdf)** — slide-deck version of the manual, regenerated on demand.
- **[Planner-Manual.pptx](Planner-Manual.pptx)** — editable PowerPoint source for the deck.
- **[_build_pptx.ps1](_build_pptx.ps1)** — script that regenerates the deck (Windows + Office; requires PowerPoint COM).

---

## Browser compatibility

| Feature                          | Chrome / Edge | Firefox       |
| -------------------------------- | ------------- | ------------- |
| Calendar, blocks, drag-drop      | ✓             | ✓             |
| Inline edits, modal, search      | ✓             | ✓             |
| CSV import, JSON export/restore  | ✓             | ✓             |
| Light / dark theme               | ✓             | ✓             |
| **Link File** auto-save to disk  | ✓             | ✗ (disabled)  |
| `localStorage` on `file://`      | ✓             | ✓ (most reliable) |

---

## Data privacy

The planner runs entirely in your browser. Nothing is ever uploaded anywhere. State lives in `localStorage` and, optionally, in a JSON file on your disk that you pick yourself. The repo's `.gitignore` excludes `planner.json` and any `planner-*.json` exports so your real scheduling data never gets pushed.

---

## License

[MIT](LICENSE) — do what you want with it.
