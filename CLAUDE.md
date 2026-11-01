# Coverage Tracker

Single-file HTML app ([index.html](index.html)) for an iPad + Apple Pencil. Dusan logs, live during a game, which defence the opponent plays and what came of it. Modelled on a Numbers sheet (blocks of Used / SF / Points / PPP).

## Decisions (and why)

- **One tap = +1** on Used, SF or Points. No menus, no aiming — it's used while coaching. A 2-point possession is 3 taps (Used, Points, Points); accepted trade-off.
- **SF = shooting foul; free-throw points count in Points**, so PPP = Points / Used stays honest.
- **Undo** reverses the last tap. **Edit mode**: rename blocks/rows (Scribble works in the text fields), add/delete rows, and tap a number to take 1 off (fixes older mistakes).
- **PPP is greyed out under 5 possessions** (`MIN_POSS`) and the footer says "small sample" — 1 possession = "2,00 PPP" means nothing and shouldn't be read as a tendency.
- **Blocks:** three PnR coverages (Weak, Show, Switch) + ISO + Post Up on one screen, 3 columns x 2 rows like the original sheet (sixth slot empty). Row names are still placeholders from the sheet.
- **Games history** kept; "New game" copies the current (edited) layout with zeros.
- Data lives in the iPad's `localStorage` (key `coverage-tracker-v2` (bumped from v1 when blocks went 4 -> 5; old v1 data was test taps only)). No server, works offline.

## Known limits / next
- localStorage is per-browser-per-address: it must always be opened from the same place on the iPad. Needs hosting (e.g. GitHub Pages) + "Add to Home Screen" to be reliable; opening a loose file from Files is not.

## Export (Export button, per game)
- **CSV** (one line per row + block totals + grand total; PPP with a dot decimal so spreadsheets read it as a number; UTF-8 BOM for Excel), **PNG picture** drawn on a canvas (not a screenshot, so it stays crisp), and **text summary** (copied to clipboard, only rows with data).
- On iPad it opens the share sheet (Files, AirDrop, Mail, Messages); elsewhere it downloads. Logic lives in the "Export" section of index.html.
- Not verified on a real iPad share sheet or in Numbers with a Serbian/Russian locale (decimal separator).

## Offline + icon
- `sw.js` (service worker) saves the app files on the iPad and serves them with no signal; when online it refreshes them in the background, so an update appears on the *next* open. Bump `CACHE` in sw.js only if the file list changes.
- Icon: `icons/icon.svg` (defender in stance) -> PNGs 180/192/512 rendered with `sharp`. iOS copies the icon at "Add to Home Screen" time, so a changed icon needs remove + re-add. The home-screen app's storage is separate from Safari's.

