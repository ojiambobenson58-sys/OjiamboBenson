# CIN001 data inventory

All datasets in this case are synthetic and intended for learning and portfolio demonstration. Currency: KES. Reporting period for the primary case: 2023-01-01 to 2025-12-31.

## Published data

| Artifact | Format | Grain / purpose | Status |
|---|---|---|---|
| `CIN001_financials_operations.csv` | CSV | 3,288 screen-day records; one screen per calendar day | Published |
| `analytical_question_bank.csv` | CSV | 160 analytical questions | Published under `documentation/` |
| `case_01_CIN001.md` | Markdown | Premium-format case brief | Published under `documentation/` |

## Source files validated but not yet published

| Artifact | Format | Rows / size | Purpose |
|---|---|---:|---|
| `CIN001_clean_sales_transactions.csv` | CSV | 41,917 rows; ~18.3 MB | Ticket-booking transaction ledger |
| `CIN001_clean_show_schedule.csv` | CSV | 10,917 unique shows; ~4.7 MB | Show-level schedule |
| `CIN001_Case1_Premium_Format_Analysis.xlsx` | Excel | ~50 KB | Monthly IMAX vs Standard analysis and assumptions |
| `CIN001_cleaning_and_analysis_summary.xlsx` | Excel | ~14 KB | Cleaning checks and reconciliations |
| `CIN001_financials_operations(1).xlsx` | Excel | ~1.66 MB | Screen-day operating and financial source workbook |

These source files were located and materialized for inspection. The full sales and show-schedule source files have not been uploaded to GitHub yet. An initial show-schedule text extraction was incomplete and was removed rather than published as if it were the full dataset.

## Grain and safe aggregation

- **Sales fact:** one group-booking transaction per row. Ticket quantities and transaction revenue can be aggregated at this grain.
- **Show schedule:** one row per unique `Show_ID`. Use this grain for show counts, attendance, capacity, and show-level occupancy.
- **Financial fact:** one screen per calendar day. Keep it separate from the transaction fact and connect through conformed dimensions.
- **Monthly analysis layer:** one row per month and screen format in the premium-format analysis workbook.

## Critical cautions

- Do not sum repeated `Show_Attendance` or `Auditorium_Capacity` values in the transaction table.
- Do not directly merge sales and screen-day financial facts; this can multiply rows and inflate totals.
- Treat rent/security/admin and salary allocation methods as explicit assumptions.
- All identifiers and results are fictional; do not present them as real customer or IMAX operational data.

## Loading recommendation

For Power BI, use separate fact tables and shared Date and Screen dimensions. For MySQL, preserve source grains in separate tables and create aggregate views for show-level and monthly reporting.
