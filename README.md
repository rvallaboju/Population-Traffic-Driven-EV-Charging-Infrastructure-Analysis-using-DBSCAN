# Population–Traffic Driven EV Charging Infrastructure Gap Analysis using DBSCAN

## Overview

This project presents a comprehensive **data-driven geospatial analysis** of Electric Vehicle (EV) charging infrastructure across the United States. The objective is to identify regions where charging infrastructure is insufficient relative to population demand and traffic congestion. By integrating multiple real-world datasets and applying spatial analytics with the **DBSCAN clustering algorithm**, the project identifies underserved regions that require infrastructure expansion.

Unlike traditional approaches that evaluate charging stations based only on station counts, this project introduces a custom **Underservice Score**, combining population demand and traffic congestion into a single infrastructure deficiency metric. Spatial clustering is then performed to determine whether underserved cities occur independently or form regional hotspot clusters.

The proposed framework provides a scalable, interpretable, and data-driven solution for supporting EV infrastructure planning, policy making, and future investment decisions.

---

# Research Question

**How can geospatial clustering techniques be used to identify regional gaps in EV charging infrastructure based on population demand and traffic congestion?**

---

# Objectives

- Analyze the distribution of EV charging stations across the United States.
- Integrate EV charging, population, and traffic datasets into a unified analytical dataset.
- Develop demand and congestion indicators representing infrastructure pressure.
- Calculate an Underservice Score to rank cities based on infrastructure deficiency.
- Apply DBSCAN clustering to identify geographically connected underserved regions.
- Visualize infrastructure gaps using geospatial mapping techniques.
- Provide actionable insights for future EV infrastructure planning.

---

# Datasets

This project combines three real-world datasets.

### 1. EV Charging Station Dataset
**Source:** U.S. Department of Energy – Alternative Fuels Data Center (AFDC)

Contains over **84,000 EV charging stations** across the United States, including:

- Station Name
- Latitude
- Longitude
- State
- City
- ZIP Code
- Level 2 Chargers
- DC Fast Chargers
- Vehicle Type
- Station Accessibility
- Network Information

---

### 2. Population Dataset

**Source:** EveryCityInTheUSA

Contains city-level population estimates for 2025 and is used to estimate charging demand.

---

### 3. Traffic Dataset

**Source:** TomTom Traffic Index

Traffic data was collected using web scraping with:

- Playwright
- Requests
- BeautifulSoup

The dataset contains:

- Traffic Congestion Percentage
- Rush Hour Delay
- City Information

---

# Project Workflow

```text
EV Charging Dataset
        │
        ▼
Data Cleaning
        │
        ▼
Population Dataset
        │
        ▼
Traffic Dataset
        │
        ▼
Dataset Integration
        │
        ▼
Feature Engineering
        │
        ▼
Demand Pressure
Congestion Pressure
        │
        ▼
Z-Score Standardization
        │
        ▼
Underservice Score
        │
        ▼
City Ranking
        │
        ▼
DBSCAN Spatial Clustering
        │
        ▼
Infrastructure Gap Analysis
```

---

# Data Preprocessing

The raw datasets underwent several preprocessing steps before analysis.

### Data Cleaning

- Removed duplicate records
- Removed unnecessary columns
- Standardized city names
- Converted ZIP codes into consistent format
- Filled missing numerical values with zero
- Filled missing categorical values with "Unknown"
- Reverse geocoded missing addresses using GeoPy
- Converted traffic percentages into numeric values
- Merged all datasets into one analytical dataset

Final cleaned dataset:

- **78,954 charging stations**
- **25 analytical features**

---

# Feature Engineering

## Demand Pressure

Demand Pressure estimates infrastructure demand relative to charging station availability.

**Formula**

```
Demand Pressure = log(1 + Population / Charging Stations)
```

Higher values indicate greater infrastructure demand.

---

## Congestion Pressure

Measures the traffic intensity surrounding charging stations.

**Formula**

```
Congestion Pressure = Traffic Congestion / Charging Stations
```

Higher values indicate more pressure on charging infrastructure.

---

# Z-Score Standardization

Since Demand Pressure and Congestion Pressure exist on different scales, both variables were standardized using Z-score normalization.

**Formula**

```
Z = (x − μ) / σ
```

Where:

- x = observed value
- μ = mean
- σ = standard deviation

This ensures both variables contribute equally when calculating the final score.

---

# Underservice Score

A weighted infrastructure deficiency score was calculated as

```
Underservice Score =
0.6 × Demand Pressure (Z-score)
+
0.4 × Congestion Pressure (Z-score)
```

Population demand was given a higher weight because charging demand primarily depends on local population.

Interpretation:

| Score | Meaning |
|---------|----------------------------|
| < 0 | Well served |
| ≈ 0 | Average infrastructure |
| > 0 | Underserved |
| >> 0 | Highly underserved |

---

# Machine Learning Model

## DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

DBSCAN was selected because it is highly suitable for geospatial datasets.

Advantages:

- No need to specify the number of clusters
- Handles irregular geographic shapes
- Detects spatial hotspots
- Identifies outliers as noise
- Robust against varying cluster sizes

---

# Distance Metric

The Haversine Distance metric was used because latitude and longitude lie on the Earth's curved surface.

Earth Radius:

```
6371 km
```

Distance conversion:

```
Distance = ε × 6371
```

For this project,

```
ε = 0.020
```

which corresponds to approximately

```
127 km
```

Cities located within approximately **127 km** of each other are considered neighbors during clustering.

---

# Hyperparameters

| Parameter | Value |
|-----------|-------|
| Algorithm | DBSCAN |
| eps | 0.020 |
| min_samples | 5 |
| Distance Metric | Haversine |
| Radius | 127 km |

---

# Model Performance

| Metric | Result |
|---------|---------|
| Number of Clusters | 13 |
| Coverage Percentage | 95.07% |
| Noise Percentage | 4.93% |
| Silhouette Score | 0.178 |

The results indicate that almost all underserved cities belong to geographically connected hotspot regions rather than isolated locations.

---

# Exploratory Data Analysis

The project includes visualizations for:

- EV Charging Station Distribution
- DC Fast Charger Distribution
- Level 2 Charger Distribution
- Vehicle Type Distribution
- Ranked Underserved Cities
- DBSCAN Cluster Map
- Top Five Infrastructure Gap Regions

---

# Technologies Used

## Programming Language

- Python

## Libraries

### Data Processing

- pandas
- numpy

### Machine Learning

- scikit-learn
- DBSCAN

### Geospatial Analysis

- geopy
- haversine

### Data Visualization

- matplotlib
- plotly
- folium

### Web Scraping

- Playwright
- BeautifulSoup
- Requests

### Utilities

- math
- warnings

---

# Repository Structure

```
EV-Charging-Infrastructure-Analysis/

│
├── data/
│   ├── EV_Charging_Data.csv
│   ├── Population_Data.csv
│   ├── Traffic_Data.csv
│
├── notebooks/
│   └── EV_Charging_Analysis.ipynb
│
├── report/
│   └── EV_CHARGING_STATION_FINAL_REPORT.pdf
│
├── presentation/
│   └── EV_Charging_Presentation.pptx
│
├── images/
│   ├── dc_fast_distribution.png
│   ├── level2_distribution.png
│   ├── ranked_cities.png
│   ├── dbscan_clusters.png
│   └── cluster_boundaries.png
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

# Key Findings

- Developed a data-driven framework to identify underserved EV charging regions across the United States.
- Integrated multiple real-world datasets, including EV charging stations, population statistics, and traffic congestion data.
- Created a custom Underservice Score combining demand and congestion indicators.
- Identified **13 geographically connected underserved regions** using DBSCAN clustering.
- Approximately **95% of underserved cities belong to regional hotspot clusters**, indicating that infrastructure shortages are regional rather than randomly distributed.
- Population demand and traffic congestion together provide a more reliable indicator of infrastructure needs than charging station counts alone.
- The proposed methodology offers an interpretable and scalable framework for future EV infrastructure planning.

---

# Future Enhancements

Potential future improvements include:

- Incorporating EV ownership statistics
- Adding demographic and socioeconomic variables
- Including road network accessibility
- Integrating real-time traffic APIs
- Time-series forecasting of charging demand
- GIS-based optimization models
- Reinforcement learning for charging station placement
- Deep learning for infrastructure demand prediction

---

# References

- U.S. Department of Energy – Alternative Fuels Data Center (AFDC)
- EveryCityInTheUSA Population Dataset
- TomTom Traffic Index
- Research papers on geospatial analytics, EV infrastructure planning, and density-based clustering referenced in the accompanying report.

---

# Author

**Ruthika Vallaboju**

Master of Science in Data Science

Regis University

---

## License

This project was developed for academic research purposes as part of the Master of Science in Data Science program at Regis University. The code, analysis, and documentation are intended for educational and research use.
