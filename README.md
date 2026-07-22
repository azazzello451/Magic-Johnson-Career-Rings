# Magic Johnson — Career Rings

A season-by-season radial visualization of Magic Johnson's NBA career, built from
[Basketball-Reference](https://www.basketball-reference.com/) game logs.

Each **ring** represents one season. Each **dot** is a single game — positioned
chronologically around the ring and colored by points scored. A short **bar**
at the end of each ring shows that season's total points, and **star markers**
mark MVP, Finals MVP, and championship honors.

Two versions are built from the same underlying geometry:

| Version | Output | Use case |
|---|---|---|
| **Static** | PNG (matplotlib) | Print, reports, portfolio images |
| **Interactive** | Standalone HTML/SVG | Hover tooltips on every game, bar, and star |

![Career Rings preview](preview.png)

*(add a screenshot of the rendered chart here before publishing)*

---

## How it works

1. **Load** — every season's game log (`.xls` / `.xlsx` / `.csv` export from
   Basketball-Reference, one file per season/game-type, e.g.
   `"1986-87 Regular Season.xls"`) is parsed and concatenated into a single
   DataFrame. Season and game type are read straight out of the filename.
2. **Bucket** — each game is sorted into a scoring bucket (`0–14`, `15–24`,
   `25+` points) that drives its marker color.
3. **Draw** — one ring per season, radiating outward from the earliest to the
   most recent. Marker size scales down slightly on inner rings to reduce
   overlap where circumference is smaller. Seasons with no matching file
   (e.g. an injury-shortened year) still get a ring — drawn thin and dashed
   instead of being silently skipped.
4. **Export** — either a high-res PNG via matplotlib, or a hand-built SVG with
   per-element `<title>` tooltips, wrapped in a standalone HTML file.

A neat detail: playoff-game markers (squares) need to line up exactly with the
season point-total bars. Rather than guessing a pixel width, the notebook
renders a probe marker off-screen and measures its actual rendered width —
`measure_marker_width_points()` — so the two always match regardless of DPI.

## Getting your own data

1. Go to a player's page on Basketball-Reference, e.g.
   [basketball-reference.com/players/j/johnsma02.html](https://www.basketball-reference.com/players/j/johnsma02.html)
2. For each season, export the **Regular Season** (and optionally
   **Playoffs**) game log as `.xls` / `.xlsx` / `.csv`
3. Save all files into one folder, keeping the season and game type in the
   filename (e.g. `"1986-87 Regular Season.xls"`, `"1986-87 Playoffs.xls"`) —
   the notebook parses these directly from the filename
4. Point `FOLDER` in the first code cell at that folder

To reuse this for a different player, edit the constants at the top of the
notebook: `PLAYER_NAME`, `TEAM_LINE`, `CAREER_STATS`, and the
`CHAMPIONSHIP_SEASONS` / `MVP_SEASONS` / `FINALS_MVP_SEASONS` sets.

## Requirements

```
pandas
numpy
matplotlib
```

Reading `.xls` files may additionally require `xlrd` or `openpyxl` depending
on the file format Basketball-Reference exports.

## Usage

Open `magic_johnson_HE2.ipynb` in Jupyter and run all cells top to bottom.

- **Part 1** builds and saves the static chart as `career_rings.png`
- **Part 2** builds the interactive version, displays it inline, and saves it
  as `magic_johnson_career.html` — open that file directly in any browser

## Notes

- Chart colors and layout constants (ring spacing, arc angle, fonts) are all
  defined near the top of each build function and are meant to be tweaked.
- `CAREER_STATS` values are entered manually in Part 1; the interactive
  version in Part 2 derives its point total automatically from the loaded
  data.

---

Part of a larger data analytics portfolio — see
[github.com/azazzello451](https://github.com/azazzello451) for more.
