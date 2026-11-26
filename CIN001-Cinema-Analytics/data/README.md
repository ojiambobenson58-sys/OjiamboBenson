# CIN001 data inventory

All datasets in this case are synthetic and intended for learning and portfolio demonstration. Currency: KES. Reporting period: 2023-01-01 to 2025-12-31.

## Identified source artifacts

| Artifact | Format | Purpose / grain | Status |
|---|---|---|---|
| `CIN001_clean_sales_transactions.csv` | CSV | Ticket-booking transaction ledger | Identified in file library; not yet committed |
| `CIN001_clean_show_schedule.csv` | CSV | One record per unique show | Identified in file library; not yet committed |
| `CIN001_Case1_Premium_Format_Analysis.xlsx` | Excel | Monthly IMAX vs Standard analysis and case assumptions | Identified in file library; not yet committed |
| `CIN001_cleaning_and_analysis_summary.xlsx` | Excel | Cleaning checks and data-quality summary | Identified in file library; not yet committed |
| `analytical_question_bank.csv` | CSV | Analytical practice questions | Identified in file library; not yet committed |

The source financial operations workbook referenced in earlier work was not confirmed in the latest file-library search. Add it only after locating and validating the exact file.

## Grain and safe aggregation
- **Sales fact:** one group-booking transaction per row. Transaction measures such as tickets sold and ticket revenue can be aggregated at this grain.
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
