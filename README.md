# Street-view and satellite greenness in Beijing

[← Zihao Lin](https://github.com/leolinnnn)

**Zihao Lin · Research manuscript · Beijing, China**

## Research question

How does the spatial extent of satellite measurement change its relationship
with street-view greenery on the same roads?

**Result:** Across 13,943 fixed paired roads, Pearson r decreases from **0.545 at 30 m to 0.312 at 100 m**. The included runner reproduces the spatial uncertainty analysis and category counts.

## Spatial comparison

<a href="assets/spatial-ranks-figure09.png"><img src="assets/spatial-ranks-figure09.png" alt="Three maps of Beijing's fixed paired roads: GVI percentile rank, 100 m NDVI percentile rank, and their signed rank difference" width="640"></a>

**Manuscript Figure 9 — spatial ranks on 13,943 paired roads.** Panels a–c show
GVI rank, 100 m NDVI rank and their difference, D = GVI rank − NDVI rank.
Positive D means a higher relative GVI rank; negative D means a higher NDVI rank.
Grey lines show unpaired framework roads. Road data: ©
[OpenStreetMap contributors](https://www.openstreetmap.org/copyright).

## Computational workflow

`Paired road measures → spatial bootstrap / Moran diagnostics → tables + verification`

- Curate street-view observations through content deduplication and road matching.
- Apply pretrained semantic segmentation and aggregate the vegetation + terrain
  fraction to roads.
- Summarise native-grid Landsat NDVI with pixel–buffer intersection-area weights.
- Keep **13,943 paired road segments** fixed when comparing 30, 50 and 100 m buffers.
- Examine correlations, within-sample ranks, median-based categories and spatial
  block-bootstrap uncertainty.

## Selected results

| Road-buffer radius | Pearson r | 95% spatial-block bootstrap CI |
| --- | ---: | --- |
| 30 m | 0.545 | 0.525–0.564 |
| 50 m | 0.451 | 0.427–0.474 |
| 100 m | 0.312 | 0.283–0.342 |

At 100 m, 5,400 roads (38.73%) have discordant high/low classifications. Between
30 and 100 m, 2,810 roads (20.15%) change category. “High” means at or above the
corresponding fixed-sample median. Intervals use 2 km blocks and 4,999 resamples.

<a href="assets/scale-association-figure07.png"><img src="assets/scale-association-figure07.png" alt="Pearson correlation decreases from 0.545 at 30 m to 0.451 at 50 m and 0.312 at 100 m, with spatial-block bootstrap confidence intervals" width="720"></a>

**Manuscript Figure 7 — scale-dependent association.** The horizontal axis is
road-buffer radius; the vertical axis is Pearson r between road GVI and NDVI.
The same roads are compared at all three scales; error bars are 95% spatial-block
bootstrap intervals. Both figures retain the original Chinese labels from the
21 October 2026 manuscript. Open either image for the full-resolution figure.

## Quick start

[Research code and run instructions](docs/IMPLEMENTATION.md) include original routines
for content deduplication, road aggregation, physical NDVI, pixel-area weighting,
spatial block bootstrap and Moran's I. The portable entry point recomputes the
spatial analysis from 13,943 paired roads and checks the archived results.

```sh
python -m pip install -r requirements.txt
python run_analysis.py --output outputs/beijing
```

Run from the repository root. The three grid sizes, 4,999 bootstrap resamples
and 999-permutation Moran diagnostic have been checked against the research
archive. Upstream image and GIS processing requires external inputs; see
[provenance and scope](PROVENANCE.md). A smaller, standard-library
[correlation check](examples/verify_beijing.py) is also retained.

## Outputs and validation

The analysis writes new files to the chosen output directory and checks the
computed results against archived reference tables. Use a new directory for each run.

| Output | Content |
| --- | --- |
| `block_bootstrap_ci.csv` | Correlations and scale differences for 1, 2 and 4 km blocks |
| `moran_rank_discordance.csv` | Rank-difference spatial diagnostics at k = 4, 8 and 12 |
| `road_categories.csv` | Per-road median categories and the 40th/60th-percentile classification |
| `verification.json` | PASS status, settings, sample size and checked counts |

Expected category counts: **5,400** discordant roads at 100 m and **2,810**
category changes between 30 and 100 m. The default run includes 4,999 bootstrap
resamples per grid and 999 Moran permutations. `--skip-moran` omits that diagnostic.

For a smaller standard-library example, run `python examples/verify_beijing.py`.

```sh
python -m pip install -r requirements-kernels.txt
python -m unittest discover -s tests -v
```

The small analytical tests are configured in [GitHub Actions](.github/workflows/checks.yml).
The full numerical reproduction is available through its manual workflow trigger.
See [implementation and validation](docs/IMPLEMENTATION.md) for verification scope.

## Repository guide

| Location | Purpose |
| --- | --- |
| [src/](src/) | Research calculation modules |
| [data/](data/README.md) | Included numerical inputs and field definitions |
| [reference/](reference/) | Archived results used after calculation for comparison |
| [examples/](examples/README.md) | Compact numerical check |
| [tests/](tests/) | Analytical edge cases for the computation kernels |
| [PROVENANCE.md](PROVENANCE.md) | Original code sources and packaging changes |
| [docs/RESEARCH_RECORD.md](docs/RESEARCH_RECORD.md) | Manuscript, figures and reuse notes |

## Measurement scope

The Beijing street-view metric combines the Cityscapes vegetation and terrain
classes. Street-view dates span 2013–2023; the Landsat scene is from 7 September
2021. Category transitions describe measurement sensitivity, not changes in real
vegetation or model accuracy.

**Tools:** Python, NumPy, GeoPandas, Shapely, rasterio/GDAL, GluonCV/MXNet.

**Manuscript:** *A Road-Segment Comparison of Street-View GVI and NDVI:
Scale-Dependent Associations and Classification Differences within Beijing's
Fifth Ring Road*.

[Source and license notes](docs/RESEARCH_RECORD.md)
