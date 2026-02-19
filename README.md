# Aviation Demand Forecasting End-to-End Pipeline  
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://www.python.org/)  
[![Prophet](https://img.shields.io/badge/Prophet-Forecasting-green)](https://facebook.github.io/prophet/)  
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  
[![Status](https://img.shields.io/badge/Status-Active-success)](https://github.com/dingjamma/aviation-demand-forecasting-end-to-end)

**End-to-end data science pipeline for aviation demand forecasting:**  
process integrity review → exploratory data analysis → feature selection → Prophet modeling → interactive deployment  

*(Synthetic data recreation of proprietary workflow – full transparency below)*

## Important Note on Proprietary Work
Since I cannot publicly share proprietary projects or internal data from my current role, I have **replicated the same end-to-end data science workflow** using **fully synthetic data that I generated myself**.  

This demonstrates the identical process and skills I apply professionally:
- Upstream process integrity review  
- Exploratory data analysis on time-series demand data  
- Critical feature selection based on quality assessment  
- Time-series forecasting with Prophet  
- Simple deployment as a shareable app  

All data, code, and insights are 100% synthetic / recreated — no real company data is used or exposed.

## Project Overview
This repository showcases a complete data science pipeline for forecasting demand in an aviation operations context (e.g., revenue hours or flight volume). The goal is to identify reliable signals for proactive resource planning and cost optimization.

### Key Components
1. Anonymized process integrity review of booking classification  
2. EDA on synthetic flight demand data (seasonality, trends, variance)  
3. Feature engineering & selection informed by process review  
4. Prophet time-series modeling (baseline + regressors)  
5. Streamlit deployment for interactive forecasting demo  

## Table of Contents
- [Process Integrity Review (Anonymized Case Study)](#process-integrity-review-anonymized-case-study)  
- [Synthetic Data Generation](#synthetic-data-generation)  
- [Exploratory Data Analysis](#exploratory-data-analysis)  
- [Feature Selection](#feature-selection)  
- [Prophet Forecasting Model](#prophet-forecasting-model)  
- [Deployment (Streamlit App)](#deployment-streamlit-app)  
- [Technologies Used](#technologies-used)  
- [How to Run Locally](#how-to-run-locally)  
- [License](#license)  

## Process Integrity Review (Anonymized Case Study)
**Objective**  
Assess alignment between documented operational booking rules and real-world practice in a fractional aviation context, to identify risks to downstream analytics, forecasting, and reporting.

**Key Observations**  
- Workflow classification often relies on unstructured free-text notes rather than deterministic, rule-based logic.  
- Eligibility thresholds (e.g., advance notice periods) have ambiguous authoritative timestamps (client contact vs. system/coordinator entry).  
- Similar business events (upgrades/downgrades) are inconsistently assigned to categories based on operational constraints rather than consistent criteria.  
- Status inheritance can cause one leg's classification to override the entire booking, losing the true business intent.  
- Certain status flags lack formal definitions, measurable triggers, or structured fields — decisions appear ad-hoc.

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

This case study is a generic, anonymized summary of the same type of analysis performed professionally — no proprietary details are disclosed.

## Synthetic Data Generation
Data is **fully synthetic** but designed to mimic realistic aviation demand patterns (trend growth, yearly seasonality, holiday peaks, daily variance, fleet-like split).

See `/notebooks/00_generate_synthetic_data.ipynb` for code.

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
Live demo: [Add link after deploying to Streamlit Cloud]

Simple interactive forecast tool:
- Select future date range  
- View predicted demand + confidence bands  
- Toggle holiday effects  

## Technologies Used
- Python 3.10+  
- pandas, NumPy, matplotlib, seaborn, Plotly  
- Prophet (facebook/prophet)  
- Streamlit (deployment)  
- Git, GitHub Pages (portfolio hosting)  

## How to Run Locally
1. Clone repo  
   ```bash
   git clone https://github.com/dingjamma/aviation-demand-forecasting-end-to-end.git
   cd aviation-demand-forecasting-end-to-end
