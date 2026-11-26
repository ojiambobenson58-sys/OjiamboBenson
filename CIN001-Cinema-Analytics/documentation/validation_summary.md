# CIN001 source-data validation summary

**Validation scope:** local inspection of the source CSV and Excel artifacts before publication. All data are synthetic.

## Record-level checks

| Dataset | Rows | Date range | Key checks |
|---|---:|---|---|
| Latest raw sales operations ledger (2026-11-18) | 42,021 | 2023-01-01 to 2025-12-31 | 42,021 unique Transaction_ID values; 41,917 Group_Booking rows and 104 Show_Only rows; 10,917 distinct Show_ID values |
| Clean sales transactions extract | 41,917 | 2023-01-01 to 2025-12-31 | All rows are Group_Booking records; all 41,917 Transaction_ID values match the raw ledger |
| Clean show schedule source | 10,917 | 2023-01-01 to 2025-12-31 | 10,917 distinct Show_ID values |
| Financial operations | 3,288 | 2023-01-01 to 2025-12-31 | 3,288 unique Record_ID values; no duplicate Date + Screen_ID keys |

The financial dataset covers 1,096 calendar days and three screens, with 1,096 records per screen. Screen S01 is IMAX; S02 and S03 are Standard.

## Sales-to-schedule reconciliation

The 104-row difference between the 42,021-row raw ledger and the 41,917-row cleaned sales extract has been explained by record type:

1. The 104 rows absent from the cleaned sales extract are all `Show_Only` records, not sales transactions.
2. All 104 have zero tickets sold, zero attendance, zero gross box-office revenue, and zero concessions revenue.
3. 92 are marked `Cancelled`; 12 are marked `Completed` with zero attendance.
4. The cleaned extract contains all 41,917 `Group_Booking` rows from the raw ledger, with no additional or missing Transaction_ID values.
5. The raw ledger contains 10,917 distinct Show_ID values, matching the 10,917 unique shows in the show schedule.

**Conclusion:** the cleaned sales extract is an appropriate sales-only fact candidate. The 104 show-only records should remain represented in the show schedule, not be appended to the transaction fact. The raw ledger should be retained as source evidence.

## Other exceptions and modelling cautions

1. The financial file has 22 missing cells, all in two margin fields. The missing values occur on 11 screen-days with zero net revenue. A margin is undefined when its denominator is zero; preserve these as blank/NULL rather than forcing them to zero.
2. The financial file contains negative operating-profit values on some screen-days. These are not automatically data errors: days with costs and no revenue can legitimately show a loss. Review the business logic and zero-revenue dates before making a performance claim.
3. The source sales ledger repeats show-level attendance and auditorium capacity across transaction rows. Do not sum those columns at transaction grain.
4. Sales and screen-day financial facts have different grains. Do not directly join them without first aggregating or using shared dimensions.

## Excel artifacts inspected

The premium-format workbook contains these sheets: START_HERE, Format_Summary, Monthly_KPIs, Data_Reconciliation, Price_Sensitivity, Efficiency_Scenario, Assumptions, Risk_Register, and PowerBI_Measures.

The cleaning and analysis workbook contains: Read_Me, KPI_Summary, Data_Quality, Missing_Values, Reconciliations, Monthly_Trends, Format_Summary, and Top_20_Movies.

## Publication note

The 3,288-row screen-day financial CSV, case brief, analytical question bank, and project documentation have been published. The cleaned sales-only extract, raw mixed sales ledger, full show-schedule source, and Excel workbooks are validated locally but are not yet published. The first attempt to transfer the show-schedule file produced an incomplete extract; that extract was removed instead of being presented as the full source.

## Recommended next validation steps

- Reconcile sales-side retained box office and concessions totals to the screen-day financial aggregates.
- Verify whether zero-revenue screen-days represent no completed screenings, cancelled screenings, or other operating conditions.
- Document the exact formulas and denominator rules for occupancy, utilization, contribution, and margin.
- Recheck all totals after importing the full source files into MySQL and Power BI.
