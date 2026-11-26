# CIN001 source-data validation summary

**Validation scope:** local inspection of the source CSV and Excel artifacts before publication. All data are synthetic.

## Record-level checks

| Dataset | Rows | Date range | Key checks |
|---|---:|---|---|
| Clean sales transactions | 41,917 | 2023-01-01 to 2025-12-31 | 41,917 unique Transaction_ID values; 10,813 distinct Show_ID values |
| Clean show schedule | 10,917 | 2023-01-01 to 2025-12-31 | 10,917 distinct Show_ID values |
| Financial operations | 3,288 | 2023-01-01 to 2025-12-31 | 3,288 unique Record_ID values; no duplicate Date + Screen_ID keys |

The financial dataset covers 1,096 calendar days and three screens, with 1,096 records per screen. Screen S01 is IMAX; S02 and S03 are Standard.

## Exceptions and items requiring reconciliation

1. The transaction ledger contains 10,813 distinct Show_ID values, while the show-schedule source contains 10,917. The difference is 104 shows. These may be scheduled shows with no booking transaction, but this must be confirmed against the source definitions before treating the difference as a definitive operational explanation.
2. The financial file has 22 missing cells, all in two margin fields. The missing values occur on 11 screen-days with zero net revenue. A margin is undefined when its denominator is zero; preserve these as blank/NULL rather than forcing them to zero.
3. The financial file contains negative operating-profit values on some screen-days. These are not automatically data errors: days with costs and no revenue can legitimately show a loss. Review the business logic and zero-revenue dates before making a performance claim.
4. The source sales ledger repeats show-level attendance and auditorium capacity across transaction rows. Do not sum those columns at transaction grain.
5. Sales and screen-day financial facts have different grains. Do not directly join them without first aggregating or using shared dimensions.

## Excel artifacts inspected

The premium-format workbook contains these sheets: START_HERE, Format_Summary, Monthly_KPIs, Data_Reconciliation, Price_Sensitivity, Efficiency_Scenario, Assumptions, Risk_Register, and PowerBI_Measures.

The cleaning and analysis workbook contains: Read_Me, KPI_Summary, Data_Quality, Missing_Values, Reconciliations, Monthly_Trends, Format_Summary, and Top_20_Movies.

## Publication note

The 3,288-row screen-day financial CSV, case brief, analytical question bank, and project documentation have been published. The full sales ledger, full show-schedule source, and Excel workbooks are not yet published. The first attempt to transfer the show-schedule file produced an incomplete extract; that extract was removed instead of being presented as the full source.

## Recommended next validation steps

- Reconcile the 104-show difference using the data-generation rules and show status definitions.
- Reconcile sales-side retained box office and concessions totals to the screen-day financial aggregates.
- Verify whether zero-revenue days represent no completed screenings, cancelled screenings, or other operating conditions.
- Document the exact formulas and denominator rules for occupancy, utilization, contribution, and margin.
- Recheck all totals after importing the full source files into MySQL and Power BI.
