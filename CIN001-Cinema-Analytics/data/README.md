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
| `CIN001_clean_sales_transactions.csv` | CSV | 41,917 rows; ~18.3 MB | Sales-only extract; one group-booking transaction per row |
| `CIN001_sales_operations(3).csv` | CSV | 42,021 rows; ~21.4 MB | Latest raw ledger; contains both 41,917 Group_Booking rows and 104 Show_Only rows |
| `CIN001_clean_show_schedule.csv` | CSV | 10,917 unique shows; ~4.7 MB | Show-level schedule source |
| `CIN001_Case1_Premium_Format_Analysis.xlsx` | Excel | ~50 KB | Monthly IMAX vs Standard analysis and assumptions |
| `CIN001_cleaning_and_analysis_summary.xlsx` | Excel | ~14 KB | Cleaning checks and reconciliations |
| `CIN001_financials_operations(1).xlsx` | Excel | ~1.66 MB | Screen-day operating and financial source workbook |

## Sales-to-schedule reconciliation

The latest raw sales ledger has 42,021 rows, but it is a mixed record set rather than a pure transaction fact:

- 41,917 rows are `Group_Booking` sales transactions.
- 104 rows are `Show_Only` records with zero tickets sold, zero attendance, zero box-office revenue, and zero concessions revenue.
- Of those 104 show-only records, 92 are marked `Cancelled` and 12 are marked `Completed` with zero attendance.
- The cleaned sales extract contains all 41,917 `Group_Booking` records and no `Show_Only` rows. Its 104-row difference from the raw ledger is therefore explained by record type, not unexplained missing sales.
- The full show schedule contains 10,917 unique `Show_ID` values. The 104 show-only records are represented at show grain and must not be appended to the sales transaction fact.

Use `CIN001_clean_sales_transactions.csv` as the sales-only candidate, subject to the remaining reconciliation and validation checks. Keep the raw mixed ledger as source evidence.

## Grain and safe aggregation

- **Sales fact:** one group-booking transaction per row. Ticket quantities and transaction revenue can be aggregated at this grain.
- **Show schedule:** one row per unique `Show_ID`. Use this grain for show counts, attendance, capacity, show-level occupancy, and cancelled/completed show counts.
- **Financial fact:** one screen per calendar day. Keep it separate from the transaction fact and connect through conformed dimensions.
- **Monthly analysis layer:** one row per month and screen format in the premium-format analysis workbook.

## Critical cautions

- Do not sum repeated `Show_Attendance` or `Auditorium_Capacity` values in the transaction table.
- Do not directly merge sales and screen-day financial facts; this can multiply rows and inflate totals.
- Treat rent/security/admin and salary allocation methods as explicit assumptions.
- All identifiers and results are fictional; do not present them as real customer or IMAX operational data.

## Loading recommendation

For Power BI, use separate fact tables and shared Date and Screen dimensions. For MySQL, preserve source grains in separate tables and create aggregate views for show-level and monthly reporting.
