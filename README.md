# NYC Taxi Data Pipeline

## Project Overview

This project examines how raw NYC yellow-taxi trip records can be
validated and prepared for reliable analytics. It uses Python to inspect
monthly trip data, identify potential data quality issues, and generate
a report with documented handling rules.

The project connects data analysis with data engineering practices,
including repeatable validation, reference-data checks, and preservation
of original records.

## Current Status

Data inspection and quality reporting are complete for January 2025.

Completed work:
- Loaded and inspected 3,475,226 trip records.
- Examined data types, missing values, and summary statistics.
- Checked trip durations, distances, and negative fares.
- Checked exact duplicate rows and pickup dates outside January.
- Validated pickup and drop-off IDs against the taxi-zone lookup table.
- Generated a consolidated quality report with planned handling actions.

Automated ingestion, data processing, SQL integration, and dashboard
development are planned.

## Data Sources

Data is provided by the
[NYC Taxi & Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

Files used:
- `yellow_tripdata_2025-01.parquet` — January 2025 yellow-taxi trip records.
- `taxi_zone_lookup.csv` — taxi-zone IDs and location descriptions.

The taxi-zone lookup file is included in this repository. The trip
Parquet file must be downloaded separately.

For field definitions, consult the Yellow Trips Data Dictionary available
on the source website.

## Repository Contents

| Path | Description |
|---|---|
| `notebooks/01_Data_Inspection.ipynb` | Data inspection, validation checks, findings, and report generation. |
| `data/raw/taxi_zone_lookup.csv` | Reference table for pickup and drop-off location IDs. |
| `reports/quality_report_2025-01.csv` | Validation counts, percentages, and planned handling actions. |

## How to Run

1. Download or clone this repository.
2. Visit the NYC TLC data page linked above.
3. Expand the 2025 download section and download January's Yellow Taxi Trip Records.
4. Place `yellow_tripdata_2025-01.parquet` in `data/raw/`.
5. Use a Python environment with Jupyter Notebook or JupyterLab.
6. Install the data-processing packages:

   ```bash
   python -m pip install pandas pyarrow
