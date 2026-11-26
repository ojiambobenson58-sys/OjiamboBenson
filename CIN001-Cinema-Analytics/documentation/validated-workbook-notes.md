# CIN001 Premium-Format Analysis Workbook — Validation Notes

**Workbook:** `CIN001_Case1_Premium_Format_Analysis_validated.xlsx`  
**Validation status:** Reconciled against the cleaned sales extract and financial operations data.  
**Data type:** Synthetic training data; not actual IMAX or cinema-company operating data.

## Corrections applied

The workbook's documentation was corrected to distinguish the mixed raw ledger from the sales-only fact:

- Raw sales ledger: **42,021 rows** and **42,021 unique Transaction_ID values**.
- Sales-only extract: **41,917 Group_Booking rows**.
- The remaining **104 rows** are `Show_Only` records: **92 Cancelled** and **12 Completed with zero attendance**. They are not sales transactions and should not be appended to the sales fact.
- The show schedule contains **10,917 unique shows**. Show-level attendance and capacity must be counted once per `Show_ID`, not summed across transaction rows.

## Sales-to-financial reconciliation

The cleaned sales fact was aggregated by `Date + Screen_ID` and compared with the financial screen-day fact.

| Measure | Sales aggregate (KES) | Financial aggregate (KES) |
|---|---:|---:|
| Gross box office | 357,880,470.00 | 357,880,470.00 |
| Distributor share | 193,643,259.96 | 193,643,259.96 |
| Cinema-retained box office | 164,237,210.04 | 164,237,210.04 |
| Concessions revenue | 79,978,085.00 | 79,978,085.00 |
| Concessions cost | 28,953,892.49 | 28,953,892.49 |

There are **3,277 matched screen-days** and **11 financial screen-days without sales rows**. All 11 unmatched days have zero revenue, zero completed screenings, and all scheduled screenings cancelled. These are consistent zero-revenue operating days.

## Modelling safeguards

1. Keep sales transactions, show schedule, and screen-day financials at their separate grains.
2. Do not directly join raw fact tables in a way that multiplies measures. Aggregate to a common grain or use conformed dimensions.
3. Preserve margin values as blank/NULL where net revenue is zero; do not replace undefined ratios with zero.
4. Negative operating profit is not automatically a data error.
5. Treat shared-cost allocation assumptions as modelling assumptions, not observed direct costs.

## Workbook correction details

The validated local workbook includes corrections to `START_HERE`, `Data_Reconciliation`, and `Assumptions`, including the corrected sales row count, the 104 `Show_Only` records, and the 3,277/11 screen-day reconciliation result.

**Binary workbook publication:** The corrected `.xlsx` file is prepared locally. This repository update records its validation details; the binary workbook itself still needs to be uploaded to this folder as `CIN001_Case1_Premium_Format_Analysis_validated.xlsx`.
