Energy-Climate Impact Analyticsace holder
Repository: mspmohle/energy-climate-impact-analytics
Stage: Production (third in AES project series)
Author: Shaun (M. Mohle)
Environment: Ubuntu 24.04 (Linux), Conda env aes-env, Jupyter Lab
Cloud Storage: AWS S3 (aes-climate-impact-analytics bucket, us-east-2)

Project Purpose

This notebook-based production pipeline analyzes the relationship between climate variability and U.S. grid reliability, integrating NOAA’s Global Historical Climatology Network v4 temperature data with DOE’s Event-Correlated Outage Dataset.
The goal is to quantify how temperature anomalies influence outage frequency and to generate reproducible analytics artifacts suitable for cloud deployment and decision-support dashboards.

Overview
The project links federal climate and energy datasets to assess temperature-driven stress on U.S. electric-grid infrastructure.
It performs ingestion, transformation, correlation, and visualization tasks while maintaining data lineage through AWS S3 synchronization and versioned local outputs.

Pipeline Summary
The production notebook (notebooks/energy_climate_impact.ipynb) performs the following stages:
Initialize AWS S3 Connectivity
Verifies access to aes-climate-impact-analytics and prepares raw/, processed/, outputs/, and logs/.
Ingest Climate and Outage Data
NOAA GHCN v4 temperature archive (.dat + .inv)
DOE Event-Correlated Outage Dataset v2 (2023)
Transform and Filter Data
Parse NOAA text files into structured DataFrames
Restrict to continental U.S. stations
Compute monthly average °C per year
Merge and Correlation Analysis
Aggregate outages by month and year
Join with NOAA temperatures
Compute temperature ↔ outage correlation (r ≈ 0.83)
Lag and Climate-Stress Index
Estimate one-month lag correlation
Build annual Climate-Stress Index (CSI) = normalized (temp × outage)
Export Artifacts
Save CSVs and PNGs to outputs/
Sync automatically to S3 under outputs/

Key Findings

Warmer months correlate strongly with increased outage frequency (r ≈ 0.83).
A one-month lag correlation of r ≈ 0.49 indicates residual climate-stress effects.
The Climate-Stress Index shows that temperature extremes consistently amplify grid stress across the continental U.S.

Repository Structure
```text
energy-climate-impact-analytics/
│
├── data/                     # Local raw data (ignored in Git)
│   ├── eia/                  # DOE outage datasets
│   └── noaa/                 # NOAA GHCN v4 archives
│
├── notebooks/
│   └── energy_climate_impact.ipynb   # Main production notebook
│
├── outputs/
│   ├── data/                 # Processed and merged CSV artifacts
│   └── figures/              # Generated plots
│
├── .gitignore
├── requirements.txt
└── README.md

Data Sources
Dataset	Description	Source URL
NOAA GHCN v4	Global Historical Climatology Network monthly temperature records	https://www.ncei.noaa.gov/pub/data/ghcn/v4/

Event-Correlated Outage Dataset v2	Aggregated U.S. power-outage reports correlated to weather events	https://data.openei.org/files/6458/Outage_Dataset_R1.zip

AWS S3 Bucket	Cloud store for processed datasets and figures	s3://aes-climate-impact-analytics/

Environment Setup
# 1. Clone repository
git clone https://github.com/mspmohle/energy-climate-impact-analytics.git
cd energy-climate-impact-analytics

# 2. Create Conda environment
conda create -n aes-env python=3.11 -y
conda activate aes-env

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter lab


Artifacts Synced to S3
Type	Example Key	Description
merged_temp_outage_2023_*.csv	outputs/data/...	Monthly correlation dataset
temp_vs_outages_*.png	outputs/figures/...	Temperature vs. outage plot
temp_lag_vs_outages_*.png	outputs/figures/...	Lag-correlation visualization
climate_stress_index_*.csv	outputs/data/...	Annual Climate-Stress Index table

Next Steps

Extend analysis to multi-year (2014–2024) trends.
Integrate EIA generation and demand data for contextual grid-stress modeling.
Explore quantum-inspired optimization (QAOA prototype) for identifying outage-prone regions under temperature anomalies.

License
© 2025 Michael Mohle
All rights reserved.  
Unauthorized copying, modification, or distribution of this repository or its contents,
via any medium, is strictly prohibited without express written permission
Maintained for professional demonstration within the WGU Data Science capstone and AES Analytics series.
