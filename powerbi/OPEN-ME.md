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
3. After the refresh, check page 1: with **2025** picked on the year slicer, the shade card reads about the README's figure for blocks; the share ending by 2045 card reads about the README's percentage. If both match, everything is joined correctly.

## The model

- **Blocks**: one row per census block (`data/geometry/blocks_wkt.csv`), keyed by `block_id`, with Plotlay's formula columns. The measures live here.
- **BlockYears**: the blocks' `canopy_2025` to `canopy_2065` unpivoted in Power Query to one row per block and year.
- **Years** (2025 to 2065): for the year slicer.
- **Streets** (`streets_wkt.csv`, with a `street` name made in Power Query), **Precincts** (`precincts_wkt.csv`) and **Trees** (`data/trees/trees.csv`).
- Measures: `Shade ha`, `Shade now ha`, `Lost by 2045 ha`, `Share lost by 2045`, `Shade per m2 of block`, `Trees`, `Trees ending by 2045`, and on Streets and Trees their own totals.

## Two map settings Power BI asks for by hand

- **Shape Map** (page 1). Select the map. In **Format visual, Visual, Map settings**, set the map type to **Custom map**, choose **Add map type**, and pick `..\data\geometry\blocks.topojson`. Under **View map keys** the block ids should match. If the Shape Map isn't in the Visualizations pane, turn it on in **Options, Preview features, Shape map visual**.
- **Azure Maps** (Map of the trees page). Your organisation's admin must allow it. For the block outlines, go to **Format visual, Reference layer**, set the data source to **File** and choose `..\data\geometry\blocks.geojson`, with join property `key`. Use the **Grayscale (light)** or **Blank** map style for screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Blocks[key]`, Color saturation `[Shade per m2 of block]`.
- Azure Maps: Latitude `Trees[lat]`, Longitude `Trees[lng]`, Legend `Trees[ule_band]`.

Then save with **File, Save**, and if you like **File, Save as** a `.pbix`, which includes the data.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Shade by decade, 2025 | Year 2025 | still |
| 2 | Shade by decade, 2055 | Year 2055: the blocks go pale | still |
| 3 | The years stepping | Click 2025, 2035, 2045, 2055, 2065, about 1 s each | 6 s clip |
| 4 | Blocks losing most | The bar chart and the table | still |
| 5 | Streets | The ranked streets | still |
| 6 | Species | The 2045 species bar and the bands | still |
| 7 | Map of the trees | Azure Maps with the block reference layer, grayscale | still |

Keep the footer with the attribution visible in every shot.
