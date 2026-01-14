# Mandi Price Intelligence & Forecasting Dashboard

## Overview
This project is an end-to-end data analytics and business intelligence system built to analyze agricultural mandi prices in India. The objective is to transform raw mandi-level price data into clear, decision-oriented insights on current price levels, short-term trends, price dispersion across mandis, and short-term price outlooks.

**Scope**
- **Commodities:** Onion, Tomato  
- **States:** Madhya Pradesh, Maharashtra, Karnataka

The workflow spans data processing in BigQuery, short-term forecasting in Python, and interactive visualization in Tableau.

---

## Problem Context
Agricultural commodity prices vary widely across mandis due to supply conditions, arrivals, logistics, and localized demand. Although mandi price data is publicly available, it is noisy, fragmented, and difficult to interpret directly at scale.

This project addresses:
1. What is the current price level and recent trend for a commodity?
2. How uneven are prices across mandis within a state?
3. Which mandis consistently offer better price realization?
4. What is the short-term directional outlook based on recent data?

---

## Data Source & Scope
The raw dataset contains mandi-level observations with:
- trade date
- state, district, and mandi name
- commodity
- minimum, maximum, and modal prices

For analytical clarity and feasibility, the data was filtered to **two commodities (Onion, Tomato)** and **three states (Madhya Pradesh, Maharashtra, Karnataka)**.

---

## Data Processing & SQL Pipeline (BigQuery)

All transformations were performed using **BigQuery SQL**, following a modular pipeline.

### 1. Data Filtering
The raw dataset was filtered by selected commodities and states to create a clean base table.

**Output:** `filtered_data`  
**Purpose:** Serves as the foundation for all downstream analysis.

---

### 2. Daily Mandi-Level Prices
Daily average prices were computed at the mandi level for each commodity and state.

**Output:** `daily_price`  
**Purpose:** Provides a clean daily time series for each mandi.

---

### 3. Rolling Features
To smooth daily volatility and capture short-term dynamics, the following were computed using SQL window functions:
- 7-day rolling average price
- 7-day rolling standard deviation
- day-over-day percentage change

**Output:** `rolling_features`

---

### 4. Price Spread Across Mandis
For each date, commodity, and state, the difference between the highest and lowest mandi prices was calculated.

**Output:** `spread_daily`  
**Purpose:** Measures price dispersion and market imbalance within states.

---

### 5. Best Mandi Identification
Mandis were compared using their 7-day average prices to identify consistently high-performing markets. One-day spikes were avoided by relying on rolling averages.

**Output:** `daily_best_mandi`

---

## Forecasting (Python)
Short-term price forecasting was implemented using Python and the Prophet library.

- **Forecast horizon:** 7 days
- **Granularity:** Selected mandi–commodity–state combinations
- **Approach:** Separate models per combination to capture local behavior

Forecast outputs were exported as CSV files for visualization and interpretation. The forecasts are intended to provide **directional insight**, not long-term predictions.

---

## Visualization (Tableau)

All insights were consolidated into an interactive Tableau dashboard.

### Dashboard Components
**Filters**
- Commodity
- State

**KPIs**
- Latest Average Price
- 7-Day Average Price
- Price Spread
- Best Mandi (7-Day Average)

**Visualizations**
- Historical price trend
- Short-term forecast panel

The dashboard enables users to explore market conditions dynamically and compare price behavior across commodities and states.

**Dashboard Link** 
https://public.tableau.com/app/profile/tejaswa.karodi/viz/Mandi_Intelligence_Dashboard1/Dashboard1?publish=yes
