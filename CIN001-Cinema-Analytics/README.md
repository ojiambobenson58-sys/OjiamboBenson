# CIN001 — Cinema Analytics

An end-to-end cinema business intelligence portfolio case study using synthetic operating and commercial data for a fictional Nairobi cinema, CIN001.

> **Data notice:** This is synthetic training data, not actual IMAX or cinema-chain operational results. Currency: KES. Period: 2023–2025.

## Business question
Does the premium IMAX format generate enough additional retained revenue and contribution to justify its higher energy and maintenance costs compared with Standard screens?

## Analytical scope
- Ticket sales, gross box office, distributor share, and exhibitor-retained box office
- Attendance, unique shows, auditorium capacity, occupancy, and screen utilization
- Concessions revenue, cost of goods sold, and gross profit
- Screen-level electricity, maintenance, and other operating costs
- Shared-cost allocation and format-level contribution
- Monthly trends, year-over-year comparisons, unit economics, and scenario analysis

## Repository structure
- `data/` — dataset inventory, grain, and loading guidance
- `documentation/` — case framing, data model cautions, and analytical methodology
- `excel/` — workbooks and validation summaries
- `sql/` — MySQL scripts
- `power-bi/` — Power BI model, DAX, and report documentation
- `reports/` — executive findings and recommendations

## Data validation
The corrected sales extract contains **41,917 Group_Booking transactions**. The raw mixed ledger contains an additional **104 Show_Only records** (92 cancelled and 12 completed with zero attendance); these are not sales transactions. The sales-to-financial reconciliation matches five monetary measures across 3,277 screen-days. The remaining 11 financial screen-days have zero revenue and all scheduled screenings cancelled.

See [validated workbook notes](documentation/validated-workbook-notes.md) for the reconciliation totals, row-grain safeguards, and correction details. The corrected Excel workbook is prepared as `CIN001_Case1_Premium_Format_Analysis_validated.xlsx`; its binary file still needs to be uploaded into the `excel/` folder.

## Modelling rules
1. Sales rows represent ticket-booking transactions, not unique shows.
2. Attendance and auditorium capacity repeat across transactions for a show. Aggregate them once per `Show_ID`.
3. Financial records have a different grain: one screen per calendar day.
4. Do not directly join sales and financial fact tables. Use conformed dimensions such as Date and Screen.
5. Shared rent, security, administration, and salary allocations are assumptions, not observed causal cost drivers.
6. CIN001 has one IMAX screen and two Standard screens; format and individual-screen effects cannot be cleanly separated.

## Interpretation limits
This is descriptive analysis for one fictional cinema over three years. Price-elasticity and efficiency-savings exercises are scenarios, not causal estimates or supplier-backed investment cases.

See [data inventory](data/README.md) and [analysis methodology](documentation/analysis-methodology.md).

## Tools
Excel • MySQL • Power BI • DAX • Data modelling • KPI design • Business analysis

## Author
Benson Ojiambo — Data Analytics Portfolio
