# CIN001 Case 1 — Premium-format dilemma

## Decision question
Does IMAX's premium-format revenue compensate for its higher operating costs when compared with Standard screenings?

## Comparison framework
Compare IMAX and Standard using the same reporting period and clearly defined measures:
- Gross box office
- Distributor settlement / share
- Retained box office revenue
- Concessions revenue, COGS, and gross profit
- Total retained revenue and contribution
- Attendance and unique completed shows
- Available and used screen hours
- Occupancy and screen utilization
- Electricity and maintenance costs
- Shared operating-cost allocations
- Revenue, contribution, and cost per attendee, show, and screen-hour

## Data model and grain
The sales ledger is transaction-grain. A show may contain multiple ticket classes and booking transactions. Show-level attendance and auditorium capacity are repeated across transaction rows, so calculate show metrics from a deduplicated show table or aggregate once per `Show_ID`.

The financial ledger is screen-day grain. Keep it as a separate fact table. Relate both facts to conformed Date and Screen dimensions rather than joining one fact directly to the other.

## Shared-cost assumptions
The supplied case analysis allocates rent, security, and administration using available screen-hours, and salaries using used screen-hours. These are transparent allocation assumptions, not verified causal cost drivers. Report the allocation basis and test alternatives as sensitivity analysis where possible.

## Confounding and limitations
CIN001 has one IMAX screen and two Standard screens. Format effects are therefore confounded with screen-specific characteristics. Results are descriptive associations for this one synthetic cinema and the 2023–2025 period; they do not establish that the format itself caused revenue or cost differences.

Price-sensitivity scenarios use assumed elasticity, and efficiency scenarios use assumed savings percentages. They are planning scenarios, not causal estimates, measured engineering savings, or vendor-backed investment proposals.

## Recommended analysis sequence
1. Validate keys, date coverage, missing values, duplicates, and expected row counts.
2. Reconcile ticket gross revenue, distributor share, and cinema-retained box office.
3. Build a unique show-level table for attendance, capacity, and show counts.
4. Reconcile concessions revenue, COGS, and gross profit.
5. Validate screen-day financial totals and preserve the financial fact grain.
6. Build monthly format-level KPIs without double-counting.
7. Compare revenue, cost, contribution, utilization, and unit economics.
8. Run sensitivity scenarios and document assumptions.
9. Translate findings into a decision with caveats, risks, and further data requirements.

## Further evidence needed for a real investment decision
A real-world decision would require multiple comparable screens and sites, actual staff rosters and time sheets, detailed maintenance logs, equipment energy-meter readings, capex and vendor quotations, contractual distributor terms, and a defensible counterfactual or matched-screen design.
