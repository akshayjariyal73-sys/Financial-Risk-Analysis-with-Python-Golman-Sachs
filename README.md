

## 🚚 Financial Risk & Customer Behavior Analysis – Goldman Sachs
A comprehensive data analytics and financial risk optimization project analyzing customer transactional behavior, wealth tiers, account volatility, and statistical risk indicators using Python.
------------------------------
## 📋 Table of Contents

* Project Overview
* Objectives
* Database & Dataset Architecture
* Data Cleaning & Preparation
* Analysis Tasks & Findings
* Key Insights & Recommendations
* KPI Metrics
* Python Code Complexity
* Files & Structure
* How to Run
* Video Walkthrough
* Author

------------------------------
## 📊 Project Overview
This project leverages advanced Python financial analytics and statistical hypothesis testing to evaluate consumer risk profiles for bank accounts. By processing multi-variable financial parameters, liquidity net flows, balance volatility index matrices, and structural data anomalies [2], the system isolates capital exposure risks, flags system-wide customer dormancy, and statistically validates high-value wealth segmentation tiers [2].
Key Focus Areas:

* 🚗 Liquidity Attrition Logs: Monitoring total monthly/yearly credit vs. debit volumes.
* 📦 Balance Volatility Analytics: Calculating coefficient of variation (CV) for tracking unstable assets [2].
* 👥 Statistical Anomaly Engines: Implementing Z-Score threshold filters to trap financial irregularities [2].
* 📍 Unsupervised Rule Tiering: Mapping consumer accounts into distinct High, Medium, and Low-value wealth brackets [2].
* 🎯 Parametric Validation Tests: Performing Welch's T-Tests and One-Way ANOVA to verify group variances [2].

Risk Analysis Dashboard: Clustering consumer assets and wealth volatility indices
------------------------------
## 🎯 Objectives

   1. Quantify Liquidity Exposures – Identify extreme deficit accounts driving large capital net outflows.
   2. Flag Structural Dormancy – Programmatically tag bank accounts inactive for consecutive 60-day windows.
   3. Isolate Capital Volatility – Uncover unstable accounts with high balance coefficients of variation.
   4. Detect Transaction Anomalies – Capture transaction spikes exceeding standard deviation thresholds via Z-Scores [2].
   5. Validate Value Segment Tiers – Statistically test whether customer tiers reflect distinct balance realities.
   6. Formulate Risk Controls – Generate algorithmic banking recommendations to protect assets from credit loss.

------------------------------
## 🗄️ Database & Dataset Architecture## Dataset File Name: goldman_sachs.csv
The analysis evaluates a transactional dataset tracking 800 unique consumer entries across 15 core financial parameters.
## Core Data Fields Schema:

| Feature Name | Purpose | Data Type | Records / Context |
|---|---|---|---|
| TransactionID | Unique sequential identifier for tracking records | Integer | 800 Unique Entries |
| CustomerID | Unique key distinguishing retail/corporate consumers | String / Object | 188 Unique Clients [2] |
| AccountID | Unique link to specific financial product accounts | String / Object | 194 Active Accounts |
| AccountType | Institutional asset divisions (Savings, Current, Credit, Loan) | Categorical | Capitalized strings [2] |
| TransactionType | Movement classification (Deposit, Withdrawal, Payment, Transfer) | Categorical | Capitalized strings [2] |
| TransactionAmount | Absolute financial value involved per record | Float | Sanitized numeric format |
| AccountBalance | Resulting ledger balance recorded post-transaction | Float | Sanitized numeric format |
| RiskScore | Continuous internal probability metrics mapping risk | Float | Normalized index |
| CreditRating | Standardized consumer credit scores | Integer | Scaled numerical bounds |
| TenureMonths | Direct duration of customer institutional relationship | Integer | Sanitized month scale |

------------------------------
## 🛠️ Data Cleaning & Preparation## Task 1.1: Financial Field Sanitization & Regex Cleaning

* Objective: Strip special character noise and cast vital ledger metrics into unified floating-point primitives.
* Method: Applied vectorized regex replacements (r'[^0-9.-]') over string representations to maintain digits, decimal boundaries, and negative boundaries.

# Programmatic regex cleaning layer for financial attributesfinancial_fields = ['TransactionAmount', 'AccountBalance', 'RiskScore', 'CreditRating', 'TenureMonths']for col in financial_fields:
    df[col] = df[col].astype(str).str.replace(r'[^0-9.-]', '', regex=True)
    df[col] = pd.to_numeric(df[col], errors='coerce')

## Task 1.2: Absolute Value Alignment & Duplicate Validation

* Objective: Eliminate absolute signs on transaction metrics and confirm unique record validity.
* Method: Executed .abs() on TransactionAmount to handle signage anomalies and verified duplicate states using .duplicated().value_counts().
* Result: Confirmed 800 valid rows with 0 duplicate rows in the final matrix [2].

## Task 1.3: Temporal Standarization & Parsing

* Objective: Validate irregular date text parameters and transform them into a unified format [2].
* Method: Processed text attributes through pd.to_datetime() with an explicit dayfirst=True argument to safely render datetime64[ns] datatypes.

# Enforcing temporal parsing and date validation
df['TransactionDate'] = pd.to_datetime(df['TransactionDate'], dayfirst=True)

## Task 1.4: Categorical Column Standardization

* Objective: Remove whitespace paddings and uneven lowercase/uppercase structures across textual markers.
* Method: Extracted object-based categories, stripped leading/trailing spaces via vector lambdas, and enforced .str.capitalize() boundaries on AccountType, TransactionType, and Region.

------------------------------
## 📈 Analysis Tasks & Findings## Task 2: Descriptive Transactional Attrition Analysis## 2.1: Temporal Net Inflow Summary

* Aggregated financial attributes by Year, Month, and Year-Month intervals to track systemic cash movement.
* Resulting Period-Volume Trends:

| Year | Credit Count | Credit Cumulative Amount | Debit Count | Debit Cumulative Amount | Net Transaction Volume | Total Frequency |
|---|---|---|---|---|---|---|
| 2023 | 135 | $7,285,849.00 | 407 | $21,399,950.00 | -$14,114,101.00 | 542 [2] |
| 2024 | 64 | $3,311,426.00 | 194 | $9,801,039.00 | -$6,489,613.00 | 258 [2] |

## 2.2: Extreme Liquidity Net Inflow Rankings

* Grouped net transaction volume strictly by AccountID to trace performance polarities.
* Findings: Top account ACC48501 generated a net positive inflow of +$346,856.34, whereas the bottom underperforming account ACC53466 generated an extreme risk exposure deficit of -$458,593.81.

## 2.3: Algorithmic Dormancy Flagging Engine

* Objective: Track and identify accounts showing severe transaction gaps of 60 days or more.
* Method: Sorted the ledger context chronologically by AccountID and TransactionDate, calculated the exact day delta between consecutive records via .diff().dt.days, and isolated records crossing the 60-day boundary.
* Result: Discovered a staggering 86.60% system-wide account dormancy rate (168 accounts categorized as Inactive/Dormant vs. only 26 accounts flagged as Active).

------------------------------
## Task 3: Multi-Variable Customer Profile Segmentation## 3.1: Activity Level Percentile Tiering

* Evaluated total transaction count frequencies per account, benchmarking metrics against the 25th and 75th percentiles to segment consumer behaviors:
* High Activity: Frequency ≥ 75th percentile (≥ 5 transactions).
   * Medium Activity: Frequency inside the middle 50% interquartile span.
   * Low Activity: Frequency < 25th percentile (< 4 transactions).

## 3.2: Custom Corporate Wealth Segmentation

* Built an exploratory classification rubric cross-evaluating customer values by matching average account balances and cumulative transaction volumes against 25th/75th percentile cuts:
* High Value Customer: Maintained top-tier average balances paired with optimal net volume trends.
   * Low Value Customer: Exhibited lower-tier average balances and heavy debit volume trends.
   * Avg Value Customer: Encompassed the remaining baseline population.

# Custom customer value classification layerdef segmentation(data):
    if data["AvgBalance"] >= Avg_High_Bal and data['TransactionVolume'] >= high_volume:
        return "High Value Customer"
    elif data["AvgBalance"] <= Avg_Low_Bal and data['TransactionVolume'] <= low_volume:
        return "Low Value Customer"
    else:
        return "Avg Value Customer"

segment_customers["Segmentation Customer Value"] = segment_customers.apply(segmentation, axis=1)

------------------------------
## Task 4: Financial Risk & Anomaly Identification## 4.1: High Withdrawal-Exposure Index

* Isolated cash depletion risks by filtering for top-quartile withdrawal counts matched with extreme cumulative outfluxes. Account ACC16241 represented the peak withdrawal vulnerability with 5 large withdrawals totaling $307,211.01.

## 4.2: Balance Volatility Analysis

* Tracked account balance consistency by computing the Coefficient of Variation ($CV = \frac{\sigma}{\mu} \times 100$) per account.
* Top 5 Erratic Volatility Brackets:

| Account ID | Standard Deviation (σ) | Mean Balance (μ) | Coefficient of Variation (CV%) | Total Balance Range |
|---|---|---|---|---|
| ACC29477 | $53,336.38 | $34,047.75 | 156.65% | $101,994.48 [2] |
| ACC70314 | $49,242.72 | $32,004.16 | 153.86% | $119,725.02 [2] |
| ACC45968 | $50,474.31 | $44,919.54 | 112.37% | $71,381.46 [2] |
| ACC21878 | $70,517.53 | $67,726.45 | 104.12% | $99,726.84 [2] |
| ACC58667 | $58,637.34 | $57,596.21 | 101.81% | $147,760.48 [2] |

## 4.3: Statistical Outlier Filtration

* Calculated continuous Z-Scores on transactional fields to isolate mathematical anomalies (|Z| > 3). Trapped 3 severe balance anomalies (e.g., ACC34821 showing an extreme Z-Score of 3.02 with a balance of $175,247.54).

------------------------------
## Task 5: Parametric Hypothesis Testing & Validation## 5.1: Welch's Independent T-Test (High vs. Low Volume Accounts)

* Hypothesis:
* $H_0: \mu_{\text{High Volume Balance}} = \mu_{\text{Low Volume Balance}}$ (Transaction frequency does not alter average balance).
   * $H_1: \mu_{\text{High Volume Balance}} \neq \mu_{\text{Low Volume Balance}}$.
* Statistical Output: t = -0.8970, p = 0.3700.
* Decision: Fail to reject H₀. Transaction frequency alone does not show a statistically significant shift in account balance levels.

## 5.2: Independent Samples T-Test (Value Segment Validation)

* Hypothesis: High-value segments hold significantly different cash assets than low-value segments.
* Statistical Output: t = 7.1630, p = 0.0000.
* Decision: Reject H₀. Proves that custom-engineered customer segments reflect highly distinct wealth profiles.

## 5.3: One-Way ANOVA Test (Multi-Tier Asset Variance)

* Hypothesis: Average asset levels differ across all three engineered value segments.
* Statistical Output: F = 17.1398, p = 0.0000.
* Decision: Reject H₀. Confirms that the customer tiers differ significantly in financial baseline metrics.

------------------------------
## 💡 Key Insights & Recommendations

* Reactivation Campaigns for the Dormant Tier: With 86.60% of accounts flagged as dormant, implement automated re-engagement workflows (e.g., targeted fee waivers, balance rewards) to recapture inactive wealth channels.
* Proactive Credit Controls for Outflow Leaders: Place accounts with massive net deficits (e.g., ACC53466 at -$458k) on strict automated risk monitoring to mitigate liquidity drain.
* Dedicated Advisory for Volatile Portfolios: Assign wealth managers to accounts exhibiting extreme Coefficient of Variation scores (CV > 150%) to help stabilize erratic capital behaviors.
* Tiered Loyalty Programs for Verified High-Value Clients: Leverage the statistically validated "High Value Customer" segment to drive asset retention and promote premium cross-selling opportunities.

------------------------------
## 🏆 KPI Metrics

* Dormancy Prevalence Index: Metric tracking the percentage of accounts exceeding the 60-day inactivity threshold.
* Balance Stability Ratio: Tracking capital predictability using the Coefficient of Variation (CV%).
* Liquidity Inflow/Outflow Spread: Spread benchmarking the difference between peak positive net inflows and bottom deficits.
* Segment Significance Lift Score: F-Statistic mapping the distinct financial boundary variances across value groups.

------------------------------
## 💻 Python Code Complexity
The project structure demonstrates modular and scalable Python data science practices:

* Advanced Data Wrangling: Utilizing lambda functions, vector mappings, regex sanitization, and advanced group aggregations via Pandas [2].
* Statistical Modeling & Testing: Executing parametric validation scripts, Z-Score outlier detection, and complex time-delta sorting engines via scipy.stats [2].

------------------------------
## 📂 Files & Structure

financial-risk-analysis/
│
├── README.md                      # Production repository documentation
├── goldman_sachs.csv              # Raw financial transaction log dataset
│
├── notebooks/                     # Analytical engine notebooks
│   └── financial_risk_analysis.ipynb # Complete clean, profile, and testing pipeline
│
└── images/                        # Exported high-resolution charts
    └── goldman-sachs-risk-distribution.png

------------------------------
## 🚀 How to Run

   1. Clone the Repository:
   
   git clone https://github.com
   cd financial-risk-analysis
   
   2. Install Required Libraries:
   
   pip install pandas numpy matplotlib seaborn scipy
   
   3. Execute the Analytics Notebook:
   Launch Jupyter and run all cells within the notebook to reproduce data cleaning, profiling, and testing steps [2]:
   
   jupyter notebook notebooks/financial_risk_analysis.ipynb
   
   
------------------------------
## 🎥 Video Walkthrough
Watch the complete technical video walkthrough detailing data wrangling steps, statistical validations, and executive financial insights:
Project Video Presentation Link [2]
------------------------------
## 👤 Author
### Akshay Jariyal
### Data Scientist & Analyst 
### 📧 Email: ajaries1997@gmail.com [2]

------------------------------
End of README file documentation.
