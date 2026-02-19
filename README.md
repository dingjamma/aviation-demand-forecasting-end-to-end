# Aviation Demand Forecasting End-to-End Pipeline  
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://www.python.org/)  
[![Prophet](https://img.shields.io/badge/Prophet-Forecasting-green)](https://facebook.github.io/prophet/)  
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  
[![Status](https://img.shields.io/badge/Status-Active-success)](https://github.com/dingjamma/aviation-demand-forecasting-end-to-end)

**End-to-end data science pipeline for aviation demand forecasting:**  
data ingestion → exploratory data analysis → feature selection → Prophet modeling → interactive deployment

## Context & Motivation

This project demonstrates the same end-to-end forecasting methodology I apply professionally in a fractional private aviation context, using publicly available data for full reproducibility.

The core business problem is the same across commercial and private operators: **forecast airborne hours 90 days out so operations can proactively plan crew rotations, fleet allocation, and maintenance windows before the schedule locks.** In fractional aviation, getting this wrong means expensive empty leg repositioning and last-minute crew scrambles. The analytical patterns — demand seasonality, fleet utilization cycles, holiday spikes — transfer directly.

Since proprietary operational data cannot be shared publicly, this pipeline uses **BTS T-100 Domestic Segment data (American Airlines, 2014–2025)** as a proxy. The data source is borrowed. The methodology is not.

## Data Source

[Bureau of Transportation Statistics T-100 Domestic Segment Dataset](https://www.transtats.bts.gov/) — filtered to American Airlines, 2014–2025. Fields used: airborne minutes, departures performed, aircraft group, year, month.

Key limitation: BTS publishes at monthly granularity. Daily-level data is disaggregated using Dirichlet sampling, which introduces synthetic variance at the day level. This is documented in the data pipeline notebook.

## Project Overview

### Key Components
1. Data ingestion and cleaning from BTS T-100 raw CSVs
2. Monthly → daily disaggregation with documented assumptions
3. EDA on 12 years of flight demand data
4. Feature engineering & selection
5. Prophet time-series modeling with COVID changepoint handling
6. Streamlit deployment for interactive forecasting demo

## Table of Contents
- [Data Pipeline](#data-pipeline)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Feature Selection](#feature-selection)
- [Prophet Forecasting Model](#prophet-forecasting-model)
- [Deployment (Streamlit App)](#deployment-streamlit-app)
- [Technologies Used](#technologies-used)
- [How to Run Locally](#how-to-run-locally)
- [License](#license)

## Data Pipeline
Notebook: `/notebooks/00_data_pipeline.ipynb`

Raw BTS T-100 CSVs (one per year, 2014–2025) are ingested from Google Drive, cleaned, and transformed into four analysis-ready tables:

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
- Strong yearly seasonality with Q4 and spring peaks
- COVID-19 structural break in 2020 with uneven post-pandemic recovery through 2022
- Weak but present day-of-week pattern
- High daily variance with identifiable outliers
- Partial 2025 data handled explicitly — not dropped, flagged in modeling

Visuals include time-series plots, seasonality decomposition, day-of-week bar charts, fleet split trends, and outlier tables.

## Feature Selection
Notebook: `/notebooks/02_feature_selection.ipynb`

| Feature | Rationale | Included? | Priority |
|---|---|---|---|
| Yearly seasonality | Strong consistent signal in EDA | Yes | High |
| US holidays | Clear demand spikes around major holidays | Yes | Medium |
| COVID period flag | Structural break — 2020–2021 distorts baseline | Yes | High |
| Fleet split by aircraft group | Demand varies by aircraft type | Optional | Medium |
| Route-level signals | Not available at this aggregation level | No | Exclude |

## Prophet Forecasting Model
Notebook: `/notebooks/03_prophet_modeling.ipynb`

- Baseline Prophet trained on 2014–2023
- US holiday regressor added
- COVID changepoint explicitly handled — model evaluated with and without 2020–2021 in training window
- Backtested on 2024 hold-out period with MAE, MAPE, and RMSE reported
- 90-day forward forecast with uncertainty intervals

## Deployment (Streamlit App)
App: `/app/app.py`  
Live demo: [Add link after deploying to Streamlit Cloud]

- Select forecast horizon
- View predicted airborne hours + confidence bands
- Toggle holiday effects on/off
- Fleet-level breakdown view

## Technologies Used
- Python 3.10+
- pandas, NumPy, matplotlib, seaborn, Plotly
- Prophet (facebook/prophet)
- Streamlit
- Google Colab + Google Drive
- Git, GitHub Pages

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
5. Launch app
```bash
   streamlit run app/app.py
```

## License
MIT
