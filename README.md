# Aviation Demand Forecasting End-to-End Pipeline  
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://www.python.org/) [![Prophet](https://img.shields.io/badge/Prophet-Forecasting-green)](https://facebook.github.io/prophet/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Status](https://img.shields.io/badge/Status-Active-success)](https://github.com/yourusername/aviation-demand-forecasting-end-to-end)

**End-to-End data science workflow for aviation demand forecasting: process integrity review → EDA → feature selection → Prophet modeling → deployment.**  
*(Synthetic data recreation of proprietary workflow – full transparency below)*

## Important Note on Proprietary Work
Since I cannot publicly share proprietary projects or internal data from my current role, I have **replicated the same end-to-end data science workflow** using **fully synthetic data that I generated myself**.  

This allows me to demonstrate the identical process and skills I apply in professional settings:
- Upstream process integrity review  
- Exploratory data analysis on time-series demand data  
- Critical feature selection based on quality assessment  
- Time-series forecasting with Prophet  
- Simple deployment as a shareable app  

All data, code, and insights are 100% synthetic / recreated — no real company data is used or exposed.

## Project Overview
This repository showcases a complete data science pipeline for forecasting demand in an aviation operations context (e.g., revenue hours or flight volume). The goal is to identify reliable signals for proactive resource planning and cost optimization.

### Key Components
1. **Process Integrity Review** — Anonymized case study on booking workflow classification issues  
2. **EDA** — Seasonality, trends, variance, and quality checks on synthetic flight demand data  
3. **Feature Engineering & Selection** — Decisions informed by process review (exclude unreliable signals, proxy where needed)  
4. **Prophet Modeling** — Baseline + regressor testing, backtesting, metrics  
5. **Deployment** — Streamlit web app for interactive forecasting demo  

## Table of Contents
- [Process Integrity Review (Case Study)](#process-integrity-review-case-study)
- [Synthetic Data Generation](#synthetic-data-generation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Feature Selection](#feature-selection)
- [Prophet Forecasting Model](#prophet-forecasting-model)
- [Deployment (Streamlit App)](#deployment-streamlit-app)
- [Technologies Used](#technologies-used)
- [How to Run Locally](#how-to-run-locally)
- [License](#license)

## Process Integrity Review (Case Study)
**Summary**  
In operational booking systems (e.g., fractional aviation), classification often depends on unstructured notes and manual overrides rather than deterministic rules. This leads to inconsistent downstream data for analytics and forecasting.

**Key Issues Identified**  
- Ambiguity in timing thresholds for eligibility (client contact vs. system entry).  
- Similar business events split across categories based on operational constraints.  
- Inheritance rules overwrite true intent (one leg's status affects entire booking).  
- Undefined priority/flag logic (ad-hoc decisions).  

**Downstream Impact**  
- Distorted demand signals  
- Unreliable fleet utilization metrics  
- Reduced forecasting accuracy  
- Manual reconciliation overhead  

**Recommendations**  
- Transition to structured classification fields  
- Define formal criteria for priority and overrides  
- Preserve "original intent" flags for analytics  

This review directly informs feature selection — unreliable or ad-hoc signals are excluded or proxied.

## Synthetic Data Generation
Data is **fully synthetic** but designed to mimic realistic aviation demand patterns (trend growth, yearly seasonality, holiday peaks, daily variance, fleet-like split).

See `/data-generation/` notebook for code.

## Exploratory Data Analysis
Notebook: `/notebooks/01_eda.ipynb`

Key findings on synthetic data:
- Strong yearly seasonality (Q4 and spring peaks)  
- Holiday spikes as clear high-demand signals  
- Weak day-of-week pattern  
- High daily variance and outliers  
- Quality checks (missing dates, zeros, duplicates)  

Visuals include time-series plots, seasonality heatmaps, DOW bars, and outlier tables.

## Feature Selection
Notebook: `/notebooks/02_feature_selection.ipynb`

Decisions informed by process review:

| Feature              | Rationale / Source                  | Included? | Priority |  
|----------------------|-------------------------------------|-----------|----------|  
| Yearly seasonality   | Strong signal in EDA                | Yes       | High     |  
| Holidays             | Proxy for peak demand periods       | Yes       | Medium   |  
| Priority-like flag   | Undefined & ad-hoc                  | No        | Exclude  |  
| Workflow type        | Inheritance issues distort intent   | No        | Exclude  |  
| Fleet split          | Potential signal (tested)           | Optional  | Medium   |  

## Prophet Forecasting Model
Notebook: `/notebooks/03_prophet_modeling.ipynb`

- Baseline Prophet trained on synthetic data  
- Added holiday regressor and tested seasonality  
- Backtested on hold-out period (MAE/MAPE/RMSE reported)  
- Forecast visualization with uncertainty intervals  

## Deployment (Streamlit App)
App: `/app/app.py`  
Live demo (after deployment): [Streamlit Cloud link – add once hosted]

Simple interactive forecast tool:
- Select future date range  
- View predicted demand + confidence bands  
- Toggle holiday effects  

Deployed free on Streamlit Cloud (public repo → auto-build).

## Technologies Used
- Python 3.10+  
- pandas, NumPy, matplotlib, seaborn, Plotly  
- Prophet (facebook/prophet)  
- Streamlit (deployment)  
- Git, GitHub Pages (portfolio hosting)  

## How to Run Locally
1. Clone repo  
   ```bash
   git clone https://github.com/yourusername/aviation-demand-forecasting-end-to-end.git
   cd aviation-demand-forecasting-end-to-end
