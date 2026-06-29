Markdown
# Enterprise Business Intelligence Solution: Sales & Strategy Performance Dashboard

## 📊 Business Case, Positioning & Impact
In enterprise environments, data silos often separate executive financial forecasting from regional operational realities. This project delivers an end-to-end Business Intelligence platform engineered to evaluate high-level organisational performance, isolate multi-year product structural contractions, and identify capital scaling risks over an $18B+ global corporate matrix.

### 💡 Business Impact Delivered:
* **Data Consolidation:** Unified disparate historical and speculative pipelines (`Actuals`, `Budgets`, and `Forecasts`) into a single source of truth, removing reporting latency across cross-functional departments.
* **Proactive Lifecycle Management:** Identified a hidden **-$634.89M revenue loss** in legacy portfolios, providing stakeholders with clear empirical proof to reallocate capital into higher-margin growth engines.
* **Automated Risk Oversight:** Deployed automated analytical governance that instantly highlights hyper-growth anomalies exceeding **50% YoY scaling**, allowing operations teams to mitigate downstream supply chain bottlenecks before they manifest.

---

## 🛠️ Data Modeling & Star Schema Architecture
A core engineering competency demonstrated in this solution is the transformation of decoupled dimensional datasets into a high-performance analytics model.

* **Fact Table:** `FactStrategyPlan` (Consolidates transactional amounts and primary indexing keys).
* **Dimension Tables:**
  * `DimProductCategory`: Establishes explicit product taxonomy and hierarchy rules.
  * `DimEntity`: Maps out parent-child relationships across regional groups, regions, and store locations.
  * `DimDate`: Custom physical calendar index driving complex time-intelligence filters.
  * `DimScenario`: Context-control matrix mapping context values (`Actual`, `Budget`, `Forecast`).

    <img width="592" height="589" alt="image" src="https://github.com/user-attachments/assets/21cfa36c-8a25-4368-8c0a-010963b9ad85" />


> **Technical Optimisation:** All relationships utilise a strict 1-to-Many ($1 \rightarrow *$) unidirectional layout flowing from dimension boundaries directly into the master fact table. This maximizes the efficiency of the VertiPaQ compression engine, prevents bidirectional filtering ambiguity, and guarantees sub-second visual rendering times.

---

## Deep-Dive Data Analytics & Core Insights

### 1. High-Level Performance Metrics
The primary health markers reveal an organisation performing significantly ahead of baseline budgets and revised projections:
* **Actual Sales:** \$18.34 Billion
* **Budgeted Sales:** \$17.17 Billion
* **Forecasted Sales:** \$18.13 Billion
* **Strategic Variance:** Actual performance outpaced budgeted baselines by **+\$1.17B (+6.81%)** while exceeding final forecasts by **+\$0.21B (+1.16%)**, confirming tight operational control and high predictive model accuracy.

### 2. The Data Paradox: Structural Collapse (Cameras & Camcorders)
* **The Findings:** While the *Cameras and Camcorders* vertical consistently met downscaled internal expectations (+5.48% variance in 2017, +11.76% in 2018, and +3.70% in 2019), trend analytics exposed a deep **-41.2% macro-market contraction**.
* **The Impact:** Absolute revenue collapsed from **\$1.54B** down to **\$905.11M** within 24 months. This insight flags systemic structural obsolescence (due to external hardware cannibalisation), validating an immediate recommendation to freeze promotional funding and execute a phased inventory rationalisation.

### 3. Hyper-Scale Expansion & Capital Governance Risks
* **The Findings:** Conversely, a secondary cross-section within the dataset exposed an emerging portfolio that underwent an unprecedented **936.6% explosion in volume**, spiking from **\$93.97M** (2018) to **\$905.11M** (2019).
* **The Impact:** While forecasting models aligned perfectly with this scaling event (a minimal 0.30% deviation), the operational safety margin relative to the budget compressed heavily from **+11.62%** down to **+3.71%**. This rapid baseline shift identifies critical downstream supply chain, logistics, and data governance overhead requiring immediate executive oversight.

---

## 💻 Technical Mastery & DAX Engineering
To translate these business insights into interactive dashboard elements, I engineered custom, performant DAX measures that avoid raw column references and optimise filter context modification.

### Single-Fact Table Scenario Isolation
Instead of requesting separate fact tables for separate data environments, I utilised `CALCULATE` to dynamically split context over the unified `FactStrategyPlan` layout:
```dax
Total Strategy Amount = SUM('FactStrategyPlan'[Amount])

Actual Sales = 
CALCULATE(
    [Total Strategy Amount],
    'DimScenario'[ScenarioName] = "Actual"
)

Budgeted Sales = 
CALCULATE(
    [Total Strategy Amount],
    'DimScenario'[ScenarioName] = "Budget"
)
Advanced Longitudinal Drift Calculations
Code snippet
Cumulative Decline vs 2017 % = 
VAR _2017_Sales = CALCULATE([Actual Sales], 'DimDate'[Year] = 2017)
VAR _Current_Sales = [Actual Sales]
RETURN 
    DIVIDE(_Current_Sales - _2017_Sales, _2017_Sales, 0)
Automated Risk Governance Rule (Hex Color Switch)
Code snippet
Growth Oversight Alert Flag = 
IF(
    [Sales YoY Growth %] > 0.50, 
    "#FF4D4D", // Automated Alert Red for high-risk scaling (>50% YoY)
    "#2A9D8F"  // Steady-state Teal for standard corporate tracking
)
