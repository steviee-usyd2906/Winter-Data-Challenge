# Fire, Heat, and the Wildlife Recovery Gap in Australia

Winter Data Analysis Challenge — The University of Sydney

**Research question:** does post-fire heat exposure widen the gap
between vegetation recovery (satellite NDVI greening) and wildlife
recovery (canopy/hollow-dependent species sightings) — i.e. does "it's
green again" become a progressively worse proxy for "the habitat is
fine" as the years after a fire get hotter?

## Report

The full analysis is one self-contained Quarto document:

- **`reports/fire_recovery_full_report.qmd`** — source
- **`reports/fire_recovery_full_report.html`** — rendered output (open this to read the report)

It builds an event-level dataset (one row per major fire, `>= 10,000 ha`)
combining fire history, NDVI, post-fire heat exposure, and indicator-
species sightings across 7 Australian states/territories, then tests
whether the gap between vegetation and wildlife recovery widens with
heat exposure (Spearman correlation, cluster-robust OLS, and a cluster
bootstrap), with an interactive map and full methodology/limitations
documented inline.

## Repository structure

```
data/                    cached CSVs + raw source data (see below)
notebooks/               exploratory/scratch notebooks (not part of the final analysis)
reports/                 the Quarto report (.qmd source + rendered .html)
requirements.txt         Python dependencies
```

### `data/`

The report reads pre-built CSVs so it renders without needing raw
shapefiles, Earth Engine access, or API keys:

| File | Contents |
|---|---|
| `fire_prone_divisions.csv` | LGA divisions meeting the fire-frequency/severity threshold |
| `fire_events_by_division_year.csv` | Fire counts/areas per division-year, incl. major fires (`>= 10,000 ha`) |
| `ndvi_annual_by_division.csv` | Annual mean NDVI per division, 1982–2022 |
| `heat_stress_by_division_year.csv` | ERA5-Land hot-day counts per division-year |
| `animal_sightings_by_division_year.csv` | Indicator-species sightings per division-year |
| `canopy_indicators_all_states.csv` | Raw ALA occurrence records (6 species, all states) |
| `au_admin2_boundaries.geojson` | LGA-level division boundaries (Earth Engine GAUL) |

Raw inputs (state fire-history shapefiles, ACORN-SAT station archives,
the AusENDVI netCDF) are gitignored due to size — the report documents
how each was processed, but those steps are marked `eval: false` and
are not needed to render the report from the cached CSVs above.

`data/animal_sightings.csv` and `data/acorn_sat_data/` are earlier,
superseded data pulls (a 3-species/3-state sightings pull and a BOM
station archive that was considered and rejected in favour of gridded
ERA5-Land data) kept for reference only — they are not read by the
current report.

### `notebooks/`

`test.ipynb` is a standalone, exploratory NDVI-animation script (not
part of the analysis pipeline or report).

## Setup

```bash
pip install -r requirements.txt
```

To render the report:

```bash
quarto render reports/fire_recovery_full_report.qmd
```

Rendering only requires the cached CSVs in `data/` (already included).
Reproducing the raw-data pipeline itself (fire shapefiles, Earth Engine
pulls, the ALA species pull) additionally needs local copies of the raw
source files, a Google Earth Engine project, and a registered ALA
(`galah`) account — see the `eval: false` appendix cells in the report
for exactly how each cached file was built.

## Authors

- Hieu Do (SID: 540914928)
- Van Duong (SID: 530745613)
- Cong Thanh Vu (SID: 530784058)
