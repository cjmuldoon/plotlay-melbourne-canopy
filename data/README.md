# Data

All files are CC BY 4.0 (see `../LICENSE-DATA` and `../NOTICE`). Coordinates are WGS 84 longitude and latitude.

## geometry/

Exported from Plotlay. Each file comes as GeoJSON (Azure Maps, QGIS, Python), TopoJSON (the Power BI Shape Map) and a
CSV with each shape's outline as WKT and its centre.

- `blocks`: the City of Melbourne's 603 CLUE blocks, `key` = `block_id`, with `area_m2` and the formula columns worked
  out in Plotlay: `canopy_2025` to `canopy_2065` (council-tree shade standing in each year, m²), `lost_by_2045`,
  `pct_lost_by_2045`, `trees` and `trees_ending_by_2045`.
- `streets`: the 1,809 road corridor segments holding a council tree, `key` = `seg_id`, with the council's `seg_descr`
  and the same columns (no tree counts).
- `precincts`: the 13 CLUE small areas, `key` = the area's name, with roll-up columns from the blocks (`canopy_2025`,
  `lost_by_2045`, `blocks`) and `pct_lost_by_2045`.

## trees/

- `trees.csv`: every council tree (`com_id`), its species, where it stands (`located_in`, `precinct`), its age class,
  `dbh_cm` where the council gives it, `ule_band` (the council's useful life band, or `Not assessed`), `end_by` (the
  year that band ends by, counted from 2025; 2066 means after 2065) and `canopy_m2` (its share of the 2021 canopy).
- `canopy_by_year.csv`: a row for each tree with shade for each year it still stands (2025, 2035, 2045, 2055, 2065).
  Linked to the blocks and streets in Plotlay by position; the year is the time axis.

How `canopy_m2` is worked out: the part of the 2021 canopy outline a tree's trunk stands in (or the nearest outline
within 3 m) that is nearer to it than to any other council tree in that outline, within 15 m of the trunk. Only Mature
and Semi-mature trees claim shade.
