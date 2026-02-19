# Aviation Demand Forecasting End-to-End Pipeline  
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://www.python.org/)  
[![Prophet](https://img.shields.io/badge/Prophet-Forecasting-green)](https://facebook.github.io/prophet/)  
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  
[![Status](https://img.shields.io/badge/Status-Active-success)](https://github.com/dingjamma/aviation-demand-forecasting-end-to-end)

**End-to-end data science pipeline for aviation demand forecasting:**  
process integrity review → exploratory data analysis → feature selection → Prophet modeling → interactive deployment  

## Data Source
This project uses **real-world public data** from the [Bureau of Transportation Statistics (BTS) T-100 Domestic Segment dataset](https://www.transtats.bts.gov/), filtered to American Airlines (2014–2025). This covers 12 years of monthly flight operations including airborne hours and departure counts across aircraft groups — serving as a public proxy for the same forecasting methodology applied in fractional/private aviation contexts.

No proprietary or internal data is used.

## Project Overview
Complete data science pipeline for forecasting aviation demand (airborne hours) with the goal of enabling proactive resource allocation and operational cost reduction.

### Key Components
1. Process integrity review of booking classification  
2. Data ingestion and cleaning from BTS T-100 raw CSVs  
3. EDA on 12 years of flight demand data (seasonality, trends, variance)  
4. Feature engineering & selection informed by process review  
5. Prophet time-series modeling (baseline + regressors)  
6. Streamlit deployment for interactive forecasting demo  

## Table of Contents
- [Process Integrity Review](#process-integrity-review)  
- [Data Pipeline](#data-pipeline)  
- [Exploratory Data Analysis](#exploratory-data-analysis)  
- [Feature Selection](#feature-selection)  
- [Prophet Forecasting Model](#prophet-forecasting-model)  
- [Deployment (Streamlit App)](#deployment-streamlit-app)  
- [Technologies Used](#technologies-used)  
- [How to Run Locally](#how-to-run-locally)  
- [License](#license)  

## Process Integrity Review
**Objective**  
Assess alignment between documented operational booking rules and real-world practice in a commercial aviation context, to identify risks to downstream analytics, forecasting, and reporting.

**Key Observations**  
- Workflow classification often relies on unstructured free-text notes rather than deterministic, rule-based logic.  
- Eligibility thresholds have ambiguous authoritative timestamps depending on when events are recorded vs. when they occur.  
- Similar business events are inconsistently categorized based on operational constraints rather than consistent criteria.  
- Status inheritance can cause one leg's classification to override the entire booking, losing the true business intent.  
- Certain status flags lack formal definitions, measurable triggers, or structured fields.

**Downstream Impact**  
- Inconsistent or distorted demand signals  
- Unreliable resource utilization and fleet metrics  
- Reduced accuracy in time-series forecasting and financial reconciliation  
- Increased manual validation overhead

**Recommendations**  
- Transition critical classification to structured, rule-enforced fields  
- Define formal criteria and expiry for ambiguous flags  
- Preserve original business intent as a separate attribute for analytics  

**How this informed the project**  
Unreliable or unstructured signals were excluded from modeling or proxied (e.g., using external holidays for known peak demand periods). This ensured the forecasting pipeline used only robust, deterministic features.

## Data Pipeline
Notebook: `/notebooks/00_data_pipeline.ipynb`

Raw BTS T-100 CSVs (one per year) are ingested from Google Drive, cleaned, and transformed into four analysis-ready tables:

| Output Table | Description |
|---|---|
| `monthly_hours.csv` | Monthly airborne hours and flight count |
| `fleet_daily.csv` | Monthly hours and flights split by aircraft group |
| `daily_hours.csv` | Daily hours disaggregated from monthly totals |
| `day_of_week.csv` | Average daily hours by day of week per year |

Key transformations: airborne minutes → hours, monthly → daily disaggregation via Dirichlet sampling, aircraft group fleet split.

## Exploratory Data Analysis
Notebook: `/notebooks/01_eda.ipynb`

Key findings:
- Strong yearly seasonality (Q4 and spring peaks)  
- COVID-19 demand collapse visible in 2020 with post-pandemic recovery  
- Weak day-of-week pattern  
- High daily variance and outliers  
- Quality checks (missing dates, zeros, duplicates)  

Visuals include time-series plots, seasonality heatmaps, DOW bars, and outlier tables.

## Feature Selection
Notebook: `/notebooks/02_feature_selection.ipynb`

| Feature | Rationale / Source | Included? | Priority |
|---|---|---|---|
| Yearly seasonality | Strong signal in EDA | Yes | High |
| Holidays | Proxy for peak demand periods | Yes | Medium |
| COVID period flag | Structural break in 2020–2021 | Yes | High |
| Fleet split by aircraft group | Potential demand signal (tested) | Optional | Medium |

## Prophet Forecasting Model
Notebook: `/notebooks/03_prophet_modeling.ipynb`

- Baseline Prophet trained on 2014–2023 data  
- Added US holiday regressor and COVID changepoint  
- Backtested on 2024 hold-out period (MAE/MAPE/RMSE reported)  
- Forecast visualization with uncertainty intervals  

## Deployment (Streamlit App)
App: `/app/app.py`  
Live demo: [Add link after deploying to Streamlit Cloud]

- Select future date range  
- View predicted demand + confidence bands  
- Toggle holiday effects  

## Technologies Used
- Python 3.10+  
- pandas, NumPy, matplotlib, seaborn, Plotly  
- Prophet (facebook/prophet)  
- Streamlit (deployment)  
- Google Colab + Google Drive (development environment)  
- Git, GitHub Pages (portfolio hosting)  

## How to Run Locally
1. Clone repo  
   ```bash
   git clone https://github.com/dingjamma/aviation-demand-forecasting-end-to-end.git
   cd aviation-demand-forecasting-end-to-end
   ```
2. Install dependencies  
   ```bash
   pip install -r requirements.txt
   ```
3. Download BTS T-100 Domestic Segment data (2014–2025) from [transtats.bts.gov](https://www.transtats.bts.gov/) and place CSVs in `data/raw/`
4. Run notebooks in order: `00` → `01` → `02` → `03`
5. Launch Streamlit app  
   ```bash
   streamlit run app/app.py
   ```

## License
MIT
