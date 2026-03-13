# Data Centers for Social Good

**Research Question:** To what extent are current U.S. data center clusters located in areas with low electricity costs and relatively favorable renewable energy conditions, and which areas appear most suitable for future responsible data center expansion under those same criteria?

## Overview

This project analyzes the geographic distribution of U.S. data centers in relation to electricity costs and renewable energy infrastructure. By combining geospatial data on data center locations with state-level electricity pricing and renewable power plant locations, the project evaluates whether existing clusters align with economically and environmentally favorable regions — and identifies candidates for responsible future expansion.

## Repository Structure

```
datacenters_for_social_good/
│
├── data_centers_cleaned.csv                  # Cleaned data center locations
├── electricity_price_cleaned.csv             # Cleaned state-level industrial electricity prices
├── plants_cleaned.csv                        # Cleaned renewable power plant locations
│
├── data_centers_preprocessing.ipynb          # Preprocessing notebook: data center dataset
├── electricity_price_preprocessing.ipynb     # Preprocessing notebook: electricity prices
├── plants_preprocessing.ipynb                # Preprocessing notebook: renewable plants
│
└── Codebook pdf.pdf                          # Full variable codebook
```

## Datasets

### 1. Data Center Locations
- **Source:** IM3 Open Source Data Center Atlas (Pacific Northwest National Laboratory / U.S. DOE)
- **Original source:** OpenStreetMap, with administrative attributes derived from U.S. Census Bureau boundaries
- **File:** `data_centers_cleaned.csv`
- **Key variables:** `id`, `state`, `county`, `lat`, `lon`, `type` (point/building/campus), `sqft`
- **Notes:** Duplicates from multi-county facilities removed; `sqft` is `NaN` for point-type entries

### 2. Electricity Prices
- **Source:** U.S. Energy Information Administration (EIA), Electric Power Monthly — Table 5.6.A
- **File:** `electricity_price_cleaned.csv`
- **Key variables:** `state`, `electricity_price` (industrial sector, cents/kWh, December 2025)

### 3. Renewable Power Plants
- **Source:** EIA Form EIA-860 (2024 release)
- **Files used:** `2___Plant_Y2024.xlsx`, `3_1_Generator_Y2024.xlsx`
- **File:** `plants_cleaned.csv`
- **Key variables:** `Plant Code`, `Plant Name`, `State`, `Latitude`, `Longitude`, `Energy Source` (SUN/WND/WAT), `Nameplate Capacity (MW)`
- **Notes:** Filtered to solar, wind, and hydropower only; ~9,000 plants with valid coordinates

## Methods

Data cleaning and preprocessing were performed in Python using `pandas` inside Jupyter Notebooks. Each dataset has a dedicated preprocessing notebook. Key steps included:

- Removing duplicate entries (multi-county data center records)
- Filtering electricity price data to industrial sector, December 2025
- Merging EIA-860 generator and plant files; deduplicating by plant code
- Retaining only variables relevant to the research question

Spatial analysis combines data center coordinates with state-level electricity prices and proximity to renewable energy plants.

## Requirements

```
python >= 3.8
pandas
jupyter
```

Install dependencies:

```bash
pip install pandas jupyter
```

## Usage

Open any of the preprocessing notebooks in Jupyter to reproduce the data cleaning steps:

```bash
jupyter notebook data_centers_preprocessing.ipynb
```

The cleaned CSVs are already provided in the repository for direct use in analysis.

## References

- Mongird, K., et al. (2026). IM3 Open Source Data Center Atlas. Pacific Northwest National Laboratory.
- U.S. Energy Information Administration (EIA). Electric Power Monthly, Table 5.6.A.
- U.S. Energy Information Administration (EIA). Form EIA-860 (2024).
