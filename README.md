# NYC Taxi Data Pipeline

## Project Overview

This project inspects 3.48 million January 2025 NYC yellow-taxi records and generates a consolidated data quality report covering missing values, trip durations, distances, fares, duplicates, and location references. SQL integration, automated ingestion, and dashboard development are planned.

## Current Status

January 2025 data inspection, quality reporting, and initial processing are complete.

## Data Sources

Source: [NYC Taxi & Limousine Commission Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

| File | Purpose |
|---|---|
| `yellow_tripdata_2025-01.parquet` | January 2025 yellow-taxi trip records. Download separately. |
| `taxi_zone_lookup.csv` | Location IDs, boroughs, and zone descriptions. Included in this repository. |

The source website also provides the Yellow Trips Data Dictionary for interpreting the fields.

## Repository Contents

| Path | Description |
|---|---|
| `notebooks/01_Data_Inspection.ipynb` | Inspection, validation checks, findings, and report generation. |
| `notebooks/02_Data_Processing.ipynb` | Reporting-period separation, quality flags, location joins, and exports. |
| `data/raw/taxi_zone_lookup.csv` | Taxi-zone reference data. |
| `reports/processing_flag_summary_2025-01.csv` | Quality-flag counts and percentages for January records.|
| `reports/quality_report_2025-01.csv` | Validation counts, percentages, and planned handling actions. |

## How to Run

1. Download or clone this repository.
2. Visit the NYC TLC data page linked above.
3. Expand the **2025** section and download January's **Yellow Taxi Trip Records**.
4. Place `yellow_tripdata_2025-01.parquet` in `data/raw/`.
5. Use a Python environment with Jupyter Notebook or JupyterLab.
6. Install the required data-processing packages:

   ```bash
   python -m pip install pandas pyarrow
   ```

7. Open `notebooks/01_Data_Inspection.ipynb` with the working directory set to `notebooks/`.
8. Run all cells in order.  
9. Open notebooks/02_Data_Processing.ipynb and run all cells in order to generate the processed datasets and flag summary.  

The notebook uses relative paths to access `../data/raw/` and writes its report to `../reports/quality_report_2025-01.csv`. Rerunning the notebook replaces the existing report.

The loaded trip table uses approximately 490.5 MB of memory. Additional memory is needed during validation.

## Key Findings

| Validation check | Flagged records | Percentage of all records |
|---|---:|---:|
| Nonpositive trip duration | 2,051 | 0.0590% |
| Nonpositive trip distance | 90,893 | 2.6155% |
| Negative fare | 144,118 | 4.1470% |
| Pickup outside January 2025 | 22 | 0.0006% |
| Exact duplicates beyond the first occurrence | 0 | 0.0000% |
| Pickup IDs missing or absent from lookup | 0 | 0.0000% |
| Drop-off IDs missing or absent from lookup | 0 | 0.0000% |

Five fields each contained 540,149 missing values, approximately 15.54% of all records:
- `passenger_count`
- `RatecodeID`
- `store_and_fwd_flag`
- `congestion_surcharge`
- `Airport_fee`

Overall, **635,232 records (18.28%) triggered at least one listed rule**. Individual rule counts overlap and should not be added together.

Inspection also identified extreme fare and distance values that require further investigation.

## Data Quality Approach

Raw files remain unchanged. Notebook 01 documents handling rules. Notebook 02 separates out-of-period records and adds quality flags and location descriptions. Other flagged January records remain in the processed dataset for review.  

| Issue | Planned handling |
|---|---|
| Missing pickup timestamp | Exclude from time-based summaries. |
| Missing drop-off timestamp | Exclude from duration and speed calculations. |
| Nonpositive duration | Exclude from duration and speed calculations. |
| Nonpositive distance | Exclude from distance and speed calculations. |
| Negative fare | Review before use in fare summaries. |
| Pickup outside January | Exclude from January-specific summaries. |
| Unmatched location ID | Exclude from the relevant geographic summaries. |
| Exact duplicate row | Review before removal. |
| Missing supplementary field | Retain the record and flag the field as unavailable. |

A flag does not automatically establish that a record is invalid. Handling depends on the intended analysis. Likewise, a location ID matching the lookup does not necessarily identify a specific known geographic location.

## Limitations

- The current analysis covers one month of yellow-taxi data.
- Validation checks identify potential issues but do not establish their causes.
- The dataset does not provide a unique trip identifier; duplicate checks compare complete rows.
- Upper-limit rules for extreme fares, distances, and durations have not yet been established.
- The current implementation requires manual notebook execution.
- SQL processing, automated ingestion, and dashboard functionality have not yet been implemented.

## Planned Enhancements

- Establish and evaluate upper-limit rules for extreme fares, distances, and durations.  
- Apply analysis-specific handling rules while preserving records for review.  
- Join pickup and drop-off location descriptions to trip records.
- Extend validation to additional months.
- Add SQL tables and analytical summaries.
- Automate ingestion with logging and safeguards against repeated loading.
- Develop a dashboard displaying analytical results and data quality measures.

## Generated Outputs

Run `01_Data_Inspection.ipynb`, followed by `02_Data_Processing.ipynb`, from the `notebooks/` folder.  

Notebook 02 automatically creates the output folders and generates:
- `data/processed/yellow_trips_2025-01_processed.parquet`
- `data/review/yellow_trips_2025-01_period_review.parquet`
- `reports/processing_flag_summary_2025-01.csv`

The Parquet outputs are not included in this repository. They can be recreated by running Notebook 02 with the downloaded source data.  Output folders do not need to exist beforehand.  

## Author

Teresa Ferrill

[Data Science Portfolio](https://ferrillt.github.io/data-science-portfolio/)
