# wildlife_health_surveillance_project
Wildlife Health Watch (WHW) is a ranger-based programme in which trained rangers report sick or dead wildlife. This project asks whether that surveillance is reaching the places where environmental stress is greatest, and how it relates to the availability of wildlife veterinary capacity across Kenya's rangelands.
## Research questions

1. Do syndromic wildlife reports show seasonal patterns, and do broad syndromes (for example sudden mortality) follow rainfall or vegetation anomalies?
2. Do reports concentrate where ranger coverage is highest rather than where environmental stress is highest? (Reporting effort is treated as a possible bias.)
3. How does distance to the nearest wildlife-capable veterinarian relate to report density?

**Not answerable with these data:** what diseases cause the reported syndromes, or true disease burden. See *Limitations*.

## Datasets

| Dataset | Status | Type | Notes |
|---|---|---|---|
| WHW syndromic reports (about 1,000, Feb 2023 onward) | In hand; cleaning script written | Tabular | Real data are held by WHW and are **not** in this repository. Only aggregated, anonymised tables are shared (see *Data privacy*). |
| CHIRPS rainfall and MODIS NDVI | Planned | Gridded raster time series | Used to calculate rainfall and vegetation anomalies against a long-term baseline. |
| Wildlife veterinary capacity layer | To be compiled | Spatial points | Built from KWS, county veterinary offices, the veterinary board register and conservancy sources; "wildlife-capable" will be defined in `docs/data_dictionary.md`. |
| Gridded Livestock of the World (FAO) | Optional | Raster | About 10 km resolution, probably too coarse for interface zones; may be used for county-level context only. |

## Repository structure

```
.
├── README.md
├── data/
│   ├── raw/         Real input data (NOT committed; see data/raw/README.md)
│   ├── sample/      Synthetic example data so the scripts can be run by anyone
│   ├── processed/   Row-level cleaned data (generated locally, NOT committed)
│   └── public/      Aggregated, anonymised tables that are safe to share
├── scripts/
│   ├── 00_make_synthetic_sample.R   Generates the fake example dataset
│   └── 01_clean_whw_reports.R       Cleans WHW reports and builds public tables
├── notebooks/       Analysis notebooks (to come)
├── results/
│   ├── tables/      Cleaning log and analysis tables
│   └── figures/     Charts and maps (to come)
└── docs/
    └── data_dictionary.md           Definitions of terms and variables
```

## How to run

1. Install R and the `tidyverse` package (`install.packages("tidyverse")`).
2. Open the project folder in R or RStudio so the working directory is the repository root.
3. Place the raw Wildlife Health Watch (WHW) export in the `raw_data/` folder as `whw_raw_live.csv`. 
4. Run `[1_data_clean.R]`.

## What the cleaning script does

The script reads the raw WHW report export and removes reports where the **Species** field is blank. I removed reports with a blank Species field because, in the data I examined, almost all of them also lacked syndrome answers and the number of animals affected. They appear to be incomplete entries, not observations of sick animals, so they contain no information about syndromes.

The script prints the number of reports before and after cleaning so the effect of the filter is visible. In the current data, 17 of 28 reports were removed.

No other changes are made to the data at this stage. Known issues still to be handled in later steps include inconsistent species names (for example, the same species recorded under more than one spelling).

## Data privacy

Row-level reports, coordinates, sites and reporter IDs are never committed. Shared tables are aggregated by county and month or quarter, and counts below 3 are hidden. 

## Limitations (so far)

- Reports are **syndromic**, not laboratory-confirmed, so one syndrome can have several causes.
- Reports reflect reporting effort as well as what is happening in wildlife.
- Facility locations are not the same as actual veterinary capacity.

## Next steps

Compile the veterinary capacity layer, download CHIRPS and NDVI for the study counties, and begin exploratory analysis.

## Definitions

**Syndromic surveillance**: tracking observed signs and patterns in animals (for example sudden death or neurological signs) without waiting for a laboratory diagnosis, so unusual events can be flagged early.

**Syndrome category**: the group a report is assigned to based on the signs described, not a confirmed cause.
Ranger-based reporting: reports submitted by trained rangers during routine field work.

**NDVI**: a satellite-derived index of vegetation greenness.

**Anomaly**: how far rainfall or NDVI in a given month departs from its long-term average for that place and season.

**Wildlife-capable veterinarian**: veterinarians authorised to practice on wildlife by the Kenya Wildlife Service
