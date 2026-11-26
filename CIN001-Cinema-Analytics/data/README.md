# CIN001 data inventory

All datasets in this case are synthetic and intended for learning and portfolio demonstration. Currency: KES. Primary case period: 2023-01-01 to 2025-12-31.

## Published data

| Artifact | Format | Grain / purpose | Status |
|---|---|---|---|
| `CIN001_financials_operations.csv` | CSV | 3,288 screen-day records; one screen per calendar day | Published |
| `analytical_question_bank.csv` | CSV | 160 analytical questions | Published under `documentation/` |
| `case_01_CIN001.md` | Markdown | Premium-format case brief | Published under `documentation/` |

## Canonical source files validated but not yet published

| Artifact | Format | Rows / size | Purpose |
|---|---|---:|---|
| `CIN001_sales_operations(3).csv` | CSV | 42,021 rows; ~21.4 MB | Latest sales source; reconciles to the premium-format workbook's row and show counts |
| `CIN001_clean_show_schedule.csv` | CSV | 10,917 unique shows; ~4.7 MB | Show-level schedule source |
| `CIN001_Case1_Premium_Format_Analysis.xlsx` | Excel | ~50 KB | Monthly IMAX vs Standard analysis and assumptions |
| `CIN001_cleaning_and_analysis_summary.xlsx` | Excel | ~14 KB | Cleaning checks and reconciliations |
| `CIN001_financials_operations(1).xlsx` | Excel | ~1.66 MB | Screen-day operating and financial source workbook |

The older `CIN001_clean_sales_transactions.csv` contains 41,917 rows and 10,813 distinct shows. It is 104 rows short of the later sales source and does not reconcile to the latest premium-format workbook. It is intentionally excluded from publication until the cleaning logic and missing rows are resolved.

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
