# Opening the Power BI report

`MelbourneCanopy.pbip` is a Power BI Project: the report (`MelbourneCanopy.Report`, PBIR format) and the semantic model (`MelbourneCanopy.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is usually shared as `\\Mac\Home`.

## Open it

1. In Windows, go to the repo's `powerbi` folder and double-click `MelbourneCanopy.pbip`.
2. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-melbourne-canopy/main/data/`). If Desktop asks how to connect, choose **Anonymous** and then **Connect**.
   - To read the files from disk instead, go to **Home, Transform data, Edit parameters** and set `DataFolder` to the full path of the repo's `data` folder, **ending with a backslash**. Then select **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** (all the sources are public open data).
3. Check the numbers (below).

If the report was updated while open, close Power BI and reopen the .pbip rather than using Apply external changes. (After Apply external changes one export drew the page at about 57% inside a much larger canvas; a full close and reopen put it right.)

## Check the numbers after Refresh

After **Refresh**, on **Shade by decade** with the Year slicer on **2025** (as the report opens; it takes one year at a time):

| Card | Should read |
|---|---|
| Shade standing in the year picked, ha | 273.4 |
| Share of 2025 shade ending by 2045 | 8.0% |

Pick the other decades and the shade card reads 271.9 (2035), 251.6 (2045), 116.4 (2055), 58.4 (2065); the column chart shows all five. The README's 274 ha counts every council tree; 273.4 ha is the part inside the 603 blocks.

**Blocks losing most**: the bar chart starts with block 931 (Parkville, 3.1 ha), then 852 (1.9 ha) and 866 (1.7 ha). **Streets**: St Kilda Road first, 5,259 m².

## The model

- **Blocks**: one row per census block (`data/geometry/blocks_wkt.csv`), keyed by `block_id`, with Plotlay's formula columns. The measures live here.
- **BlockYears**: the blocks' `canopy_2025` to `canopy_2065` unpivoted in Power Query to one row per block and year.
- **Years** (2025 to 2065): for the year slicer. The shade measures always use one decade: the one picked, or 2025 when none is.
- **Streets** (`streets_wkt.csv`, with a `street` name made in Power Query), **Precincts** (`precincts_wkt.csv`) and **Trees** (`data/trees/trees.csv`).
- Measures: `Shade ha`, `Shade now ha`, `Lost by 2045 ha`, `Share lost by 2045`, `Shade per m2 of block`, `Trees`, `Trees ending by 2045`, and on Streets and Trees their own totals.

## The two maps

- **Shape Map** (page 1). The custom map, `data/geometry/blocks.topojson`, is saved inside the report (`MelbourneCanopy.Report/StaticResources/RegisteredResources/blocks.topojson`, a copy that keeps only the block id), so the blocks draw on open, shaded by shade per m² of block for the year picked. The Shape Map has no base map of its own, so the file also carries the context: the suburbs round the city (ABS, CC BY 4.0) in a pale ground tone, with the Yarra, Victoria Harbour and the bay showing as the map's pale blue background (their outline from Vicmap Hydro, CC BY 4.0). Loading the plain file from `data/` by hand (below) gives the blocks without that context. If the map shows the United States instead (an older Desktop can ignore a saved map): select it, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\blocks.topojson`. Under **View map keys** the block ids (244, 931...) should be matched. If the Shape Map is not in the Visualizations pane at all, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (Map of the trees page). It opens on a light grey street map zoomed to the trees. It shows the 4,946 council trees whose useful life ends by 2045 (a visual filter on `end_by`): all 82,064 trees is more points than Azure Maps plots. Each tree is a point: Location is the tree id, latitude and longitude are averaged (one point per tree), the legend is the useful life band and the species is in the tooltip. Your organisation's admin must allow Azure Maps (on by default on a personal account). For the block outlines add the reference layer by hand (Power BI can't take it from a file in the project): **Format visual**, **Reference layer**, **Data source: File** (or **URL**), and choose `..\data\geometry\blocks.geojson`. Use the **Grayscale (light)** or **Blank** map style for screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Blocks[key]`, Color saturation `[Shade per m2 of block]`.
- Azure Maps: Location `Trees[com_id]`, Latitude `Trees[lat]` (Average), Longitude `Trees[lng]` (Average), Legend `Trees[ule_band]`.

Then save with **File, Save**, and if you like **File, Save as** a `.pbix`, which includes the data.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/Screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Shade by decade, 2025 | Year 2025 (as it opens) | still |
| 2 | Shade by decade, 2055 | Year 2055: the blocks go pale | still |
| 3 | The years stepping | Pick 2025, 2035, 2045, 2055, 2065 in the Year dropdown, about 1 s each | 6 s clip |
| 4 | Blocks losing most | The bar chart and the table | still |
| 5 | Streets | The ranked streets | still |
| 6 | Species | The 2045 species bar and the bands | still |
| 7 | Map of the trees | Azure Maps (trees ending by 2045) with the block reference layer, grayscale | still |

Keep the footer with the attribution visible in every shot.
