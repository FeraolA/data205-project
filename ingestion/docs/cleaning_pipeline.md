# Ingestion Pipeline

**Notebook:** `ingestion/notebooks/1.0-data-ingestion-cleaning.ipynb`

## Overview

This pipeline loads both raw datasets from the Montgomery County Open Data API and transforms them into clean, analysis-ready files saved to Google Drive. No manual file download is required -- all data is fetched programmatically via the Socrata API.

---

## Data Sources

| Field | Value |
|-------|-------|
| Dataset | Traffic Violations |
| Source | Montgomery County Open Data Portal |
| Records loaded | 150,000 most recent (ordered by date DESC) |
| Variables | ~35 columns |
| API endpoint | data.montgomerycountymd.gov/api/v3/views/4mse-ku6q/query.csv |

| Field | Value |
|-------|-------|
| Dataset | Parking Garage and Lot Inventory |
| Source | Montgomery County Open Data Portal |
| Records | ~200 rows |
| API endpoint | data.montgomerycountymd.gov/api/v3/views/rd7s-ntxu/query.json |
| Purpose | Geographic context alongside violation location data |

---

## Step 1 -- Load Raw Data

Both datasets are pulled directly from the Socrata API using `pd.read_csv()`. Raw files are saved to `data/raw/` without modification. The raw files must never be edited.

---

## Step 2 -- Clean Traffic Violations

| Step | What it does |
|------|-------------|
| Column names | Stripped, lowercased, spaces replaced with underscores |
| Date parsing | `date_of_stop` parsed as datetime; `time_of_stop` parsed as time object |
| Temporal features | Derived: `year`, `month`, `month_name`, `day_of_week`, `hour`, `time_period` |
| Boolean conversion | Yes/No fields (`accident`, `alcohol`, `search_conducted`, etc.) converted to 0/1 integers |
| Categorical cleanup | `violation_type`, `race`, `gender`, `arrest_type` standardized to Title Case; `'Nan'` replaced with NaN |
| Coordinate fix | Latitude and longitude values of 0.0 set to NaN (invalid GPS readings) |
| Row filter | Rows with no date dropped; dataset filtered to 2018 onward |

---

## Step 3 -- Clean Parking Inventory

- Column names standardized to lowercase with underscores
- Empty strings replaced with NaN
- Rows with no location value dropped

---

## Output Files

| File | Location |
|------|----------|
| `traffic_violations_raw.csv` | `data/raw/` -- original download, never modified |
| `traffic_violations_clean.csv` | `data/processed/` -- cleaned, filtered to 2018+, ~212,000 rows |
| `parking_inventory_clean.csv` | `data/external/` -- cleaned parking inventory |

---

## Large File Handling

The full Traffic Violations dataset exceeds GitHub's 100 MB file size limit. The raw file is not stored in the repository. Access instructions and direct download links are in `data/raw/DATA_ACCESS.md`. The notebook loads the data directly from the API so no manual download is needed to reproduce the analysis.
