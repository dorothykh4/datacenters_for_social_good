# Data Centers for Social Good

**Research Question:** To what extent are current U.S. data center clusters located in areas with low electricity costs and relatively favorable renewable energy conditions, and which areas appear most suitable for future responsible data center expansion under those same criteria?

## Overview

This project analyzes the geographic distribution of U.S. data centers in relation to electricity costs, renewable energy infrastructure, environmental constraints, and social equity. By combining geospatial data on data center locations with state-, facility-, and county-level datasets, the project evaluates whether current clusters align with economically and environmentally favorable regions, and where future expansion could be more responsible.

The analysis uses a multi-method framework. First, state-level OLS regression tests whether electricity price, renewable capacity, and renewable proximity are associated with broader patterns of data center concentration. Second, facility-level logistic regression examines whether individual locations with cheaper electricity and closer renewable infrastructure are more likely to contain a data center. Third, county-level random forest modeling identifies the most important predictors of current data center presence, including density, renewable access, vulnerability, and water stress. Finally, a simplified mixed-integer linear programming (MILP) optimization model uses these insights to recommend counties for future data center siting that balance energy opportunity with lower environmental and community burden.

Together, the project moves beyond describing where data centers are today. It asks whether historical siting patterns reflect the best available locations, and how future digital infrastructure growth could better align with affordability, clean energy, resilience, and equity.

## Repository Structure

```
datacenters_for_social_good/
│
├── data_centers_cleaned.csv                  # Cleaned data center locations
├── electricity_price_cleaned.csv             # Cleaned state-level industrial electricity prices
├── plants_cleaned.csv                        # Cleaned renewable power plant locations
├── county_features.csv                       # County-level modeling dataset
│
├── data_centers_preprocessing.ipynb          # Preprocessing notebook: data center dataset
├── electricity_price_preprocessing.ipynb     # Preprocessing notebook: electricity prices
├── plants_preprocessing.ipynb                # Preprocessing notebook: renewable plants
├── eda_prices.ipynb                          # EDA of data centers and electricity prices
├── eda_renewables.ipynb                      # EDA of renewable plants
├── regression.ipynb                          # Runs OLS and logistic regressions
├── county_features.ipynb                     # Builds county-level feature table
├── modeling.ipynb                            # Runs random forest and MILP optimization
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

### 4. U.S. County Boundaries
- **Source**: U.S. Census Bureau TIGER/Line Cartographic Boundary Files (2023)
- **Original source**: United States Census Bureau
- **File**: cb_2023_us_county_20m.zip
- **Key variables**: `GEOID`, `NAME`, `STATEFP`, `COUNTYFP`, `geometry`
- **Notes**: Simplified county polygon shapefile used for spatial joins, county-level aggregation, and mapping. Includes all U.S. counties and county-equivalent areas.

## Methods

Data cleaning, preprocessing, and modeling were conducted in Python within Jupyter Notebooks using pandas, geopandas, scikit-learn, statsmodels, and PuLP. Each raw dataset was cleaned separately and then integrated into state-, facility-, and county-level analytical datasets.

The project applies a multi-method framework:

- **Exploratory Data Analysis (EDA)**: maps, rankings, correlations, and descriptive summaries of U.S. data center geography
- **State-level OLS Regression**: tests associations between data center concentration, electricity price, renewable capacity, and renewable proximity
- **Facility-level Logistic Regression**: estimates the probability of data center presence at individual locations versus comparison sites
- **Random Forest Classification**: identifies the most important county-level predictors of existing data center presence
- **MILP Optimization**: selects counties for future data center expansion under energy, equity, environmental, and diversification constraints

This workflow combines geospatial, economic, environmental, and social data to evaluate both where data centers are currently located and where future growth may be more sustainable and equitable.

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

## License

This project is released under the Creative Commons Attribution 4.0 International (CC BY 4.0) license. You are free to share, copy, redistribute, adapt, and build upon the materials in this repository for any purpose, including commercial use, provided that appropriate credit is given to the original author.

Please cite or acknowledge this project if you reuse the code, analysis, visualizations, or written materials.

Third-party datasets used in this project remain subject to their own respective licenses and terms of use, including data provided by the U.S. Energy Information Administration (EIA), Pacific Northwest National Laboratory (PNNL), OpenStreetMap contributors, and other original sources. Users are responsible for reviewing and complying with those source-specific terms.

Full license text: https://creativecommons.org/licenses/by/4.0/
