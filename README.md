# Corporate Sales Performance BI Intelligence

An end-to-end Power BI solution analyzing an $18B+ retail dataset (Microsoft's Contoso sample data). Built on a single-fact-table star schema, with custom DAX measures for variance analysis, multi-year time intelligence, and automated growth-risk flagging.

## Overview

This project evaluates organisational sales performance across the 2017–2019 fiscal periods — covering high-level performance against budget and forecast, product category contribution, year-over-year trend analysis, and two category-level deep dives.

![Main Dashboard](./screenshots/main-dashboard.png)
*Main dashboard — KPI cards, Budget Product Sold, and Budgeted Monthly Amount by category, 2017–2019.*

## Data Model

Star schema with the following tables:

- `DimAccount`
- `DimDate`
- `DimEntity`
- `DimProductCategory`
- `DimScenario`
- `FactStrategyPlan`

All calculations live in a dedicated `_Calculated Measures` folder, keeping the model clean and the logic auditable.

## DAX Measures

| Measure | Purpose |
|---|---|
| `Total Strategy Amount` | Base aggregation — `SUM(FactStrategyPlan[Amount])` |
| `Actual Sales` / `Budgeted Sales` / `Forecasted Sales` | Scenario-filtered variants of Total Strategy Amount via `DimScenario[ScenarioName]` |
| `Budget Variance` / `Budget Variance %` | Actual vs. Budget, using `DIVIDE()` to guard against divide-by-zero |
| `Forecast Variance %` | Actual vs. Forecast, same `DIVIDE()` safeguard |
| `Sales YoY Growth %` | Year-over-year comparison using `SELECTEDVALUE` on the Year column (chosen over `SAMEPERIODLASTYEAR` for robustness against a non-contiguous date table) |
| `% Change vs 2017 Baseline` | Variance of the current filter context against a fixed 2017 baseline year |
| `Growth Oversight Alert Flag` | Returns a hex colour (`#FF4D4D` / `#2A9D8F`) consumed directly by conditional formatting, flagging any category with >50% YoY growth |

## Key Findings

**High-level performance:** Actual sales of $18.34bn beat budget by +6.81% and forecast by +1.16%, indicating accurate, slightly conservative forecasting.

**Cameras and Camcorders — structural decline:** Revenue fell from $1,538.40M (2017) to $905.11M (2019), a -41.2% contraction, consistent with market cannibalisation from smartphone cameras. The category still beat its own downscaled budget every year — a reminder that "beating budget" and "growing" are not the same thing.

**Audio — sustained high growth:** Audio grew +53% in 2018 and a further +25% in 2019, nearly doubling in absolute revenue over two years ($61.27M → $117.84M). The `Growth Oversight Alert Flag` measure correctly surfaced Audio as the only category crossing the 50% YoY growth threshold in 2018.

![Audio Deep Dive](./screenshots/audio-deep-dive.png)
*Deep Dive page, 2018 — Audio flagged red by the Growth Oversight Alert Flag after posting +53% YoY growth.*

## Data Validation

An earlier version of this project's analysis misattributed 2019 figures from Cameras and Camcorders to a different category, producing an inflated and incorrect growth claim. This was identified during review, traced back to the source data, and corrected — see [Issue #1](../../issues/1) for the full record of what was flagged and how it was resolved. All figures in this README and the accompanying report have been re-verified directly against the Power BI data model.

## Report

The full written analysis, including deep-dive tables and strategic recommendations, is available here: [Consolidated Executive Sales Analysis Report](./Consolidated_Executive_Sales_Analysis_Report_Corrected.docx)

## Tools

Power BI Desktop · DAX · Star schema data modelling
