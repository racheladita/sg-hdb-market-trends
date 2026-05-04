# Singapore HDB Market Trends Analysis

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.x-013243?logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.x-11557c)](https://matplotlib.org/)

A data analysis project that investigates long-term trends in the Singapore HDB (Housing & Development Board) resale market using **NumPy** and **Matplotlib**. The analysis covers resale price index movements, unit sales vs rental volume dynamics, and the interplay between pricing and transaction volume across a 16-year period (2006–2021).

## 📊 Analysis Pipeline

```mermaid
graph TD
    subgraph Data Ingestion
        D1[(HDB Residential Unit Data<br/>334 records, 5 features)] --> D2[NumPy genfromtxt]
        D3[(HDB Quarterly Resale<br/>Price Index<br/>143 records, 2 features)] --> D2
    end

    subgraph Preprocessing
        D2 --> P1[Handle Missing Values<br/>Replace 'na' → 0]
        P1 --> P2[Type Conversion<br/>String → Integer/Float]
        P2 --> P3[Boolean Indexing<br/>Filter by Category: Sold/Rented]
        P3 --> P4[Yearly Aggregation<br/>Sum units per year]
        D2 --> P5[Extract Year from Quarter]
        P5 --> P6[Compute Average Index per Year]
    end

    subgraph Visualization & Analysis
        P4 --> V1[Line Chart: Sold vs Rented Units]
        P6 --> V2[Line Chart: Yearly Average Price Index]
        P4 --> V3[Dual-Axis Chart:<br/>Units Sold vs Price Index]
        P6 --> V3
    end

    subgraph Key Findings
        V1 --> F1([Sales peaked at 28,739 units in 2017])
        V2 --> F2([Price index rose sharply 2007-2013<br/>then moderated until COVID-19 rebound])
        V3 --> F3([Sales and prices do not always<br/>move in lockstep])
    end

    style D1 fill:#f96,stroke:#333,stroke-width:2px,color:#000
    style D3 fill:#f96,stroke:#333,stroke-width:2px,color:#000
    style F1 fill:#9f9,stroke:#333,stroke-width:2px,color:#000
    style F2 fill:#9f9,stroke:#333,stroke-width:2px,color:#000
    style F3 fill:#9f9,stroke:#333,stroke-width:2px,color:#000
```

## 📖 Project Overview

The Singapore HDB resale market is one of the most significant public housing markets in the world. This project analyzes two publicly available datasets to uncover trends in transaction volume and pricing, and to identify correlations (or lack thereof) between these metrics over time.

The analysis is conducted entirely using low-level data manipulation with **NumPy** arrays and visualization with **Matplotlib**, demonstrating proficiency in fundamental numerical computing.

## 🛠️ Methodology

### Data Preprocessing

1. **Data Loading**: Both CSV files are loaded using `numpy.genfromtxt()` with header skipping, stored as string-typed NumPy arrays for initial processing.
2. **Missing Value Handling**: The `no_of_units` column contains `'na'` entries, which are replaced with `'0'` using `numpy.where()` before integer conversion.
3. **Type Conversion**: Unit counts are cast to `int`, price indices to `float`.
4. **Boolean Indexing**: Records are filtered by category (`Sold` / `Rented`) using NumPy boolean array operations to compute per-year aggregates.
5. **Temporal Aggregation**: Quarterly price index data is grouped by year (extracting the first 4 characters of the quarter string) and averaged to produce yearly price index values.

### Visualization Approach

| Chart                          | Type              | Purpose                                                                                                        |
| :----------------------------- | :---------------- | :------------------------------------------------------------------------------------------------------------- |
| **Sold vs Rented Units**       | Dual-line chart   | Compare transaction volume trends between the two categories over time                                         |
| **Yearly Average Price Index** | Single-line chart | Track long-term price index movement with quarterly noise smoothed out                                         |
| **Units Sold vs Price Index**  | Dual-axis chart   | Overlay sales volume (left y-axis) against price index (right y-axis) to identify correlations and divergences |

## 🧠 Key Findings

- **Sales Volume Volatility**: The number of units sold exhibits significant year-over-year volatility, ranging from ~4,700 (2008) to ~28,700 (2017), driven by policy changes, economic conditions, and supply constraints.
- **Rental Market Stability**: Rental volumes remain remarkably stable (2,700–4,900 units/year), indicating that the rental segment serves as a consistent fallback during periods of high resale prices or limited supply.
- **Price-Volume Divergence**: Sales and prices do not always move in lockstep. During 2009–2013, both rose simultaneously (strong demand). In 2014, sales surged while prices stabilized, suggesting non-price factors (eligibility rules, loan accessibility) drive buyer behavior.
- **COVID-19 Impact**: The 2020–2021 period shows simultaneous increases in both sales volume and price index, attributed to BTO construction delays pushing demand into the resale market.
- **Policy Sensitivity**: Cooling measures (stricter loan eligibility, higher stamp duties) are visible in the data as periods of price moderation between 2015–2019.

## ⚙️ Backend Integration Potential

This analysis pipeline is well-suited for integration into a backend data service:

- **Automated Data Ingestion**: The NumPy-based CSV parsing can be replaced with scheduled API calls to [data.gov.sg](https://data.gov.sg/) for live HDB transaction data, feeding into a backend data pipeline.
- **REST API for Market Analytics**: The aggregation and trend computation logic can be wrapped in a FastAPI/Flask endpoint, serving real-time market trend data to frontend dashboards or mobile applications.
- **Time-Series Forecasting Extension**: The yearly aggregated data provides a clean foundation for integrating time-series forecasting models (e.g., ARIMA, Prophet) into a prediction microservice.

## 📊 Datasets

### HDB Residential Unit Data

Source: [data.gov.sg](https://data.gov.sg/)

- **334 records** with **5 columns**
- Covers financial years 2006–2021

| Column           | Type | Description                |
| :--------------- | :--- | :------------------------- |
| `financial_year` | int  | Year of transaction        |
| `property_type`  | str  | Always "HDB"               |
| `category`       | str  | Sold / Rented              |
| `flat_type`      | str  | 1-room to Executive flats  |
| `no_of_units`    | int  | Number of units transacted |

### HDB Quarterly Resale Price Index

Source: [data.gov.sg](https://data.gov.sg/)

- **143 records** with **2 columns**
- Covers 1990-Q1 to 2024-Q4

| Column    | Type  | Description                    |
| :-------- | :---- | :----------------------------- |
| `quarter` | str   | Year-Quarter (e.g., "2020-Q3") |
| `index`   | float | Resale price index value       |

## 🚀 Requirements

```bash
pip install numpy matplotlib
```

## 📂 Repository Structure

```
sg-hdb-market-trends/
├── sg_hdb_market_trends.ipynb          # Analysis notebook
├── hdb_residential_unit_data.csv       # HDB unit sales/rental data
└── hdb_quaterly_resale_price_index.csv # Quarterly resale price index
```

---

_Developed by Adita Putri Puspaningrum._
