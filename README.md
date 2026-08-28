# Telecom Network Quality Analysis

A Power BI dashboard analyzing call quality across telecom operators in India, built from 620 crowdsourced network readings.

## Overview

This project explores call drop rates, user ratings, network type performance, and usage context (indoor/outdoor/travelling) across 5 telecom operators — BSNL, RJio, VI, Airtel, and MTNL — to understand where and why network quality breaks down.

## Dataset

- **Source:** MyCall-style crowdsourced call quality dataset (January 2022)
- **Size:** 620 rows, 8 columns
- **Columns:** operator, inout_travelling (indoor/outdoor/travelling), network_type (2G/3G/4G/Unknown), rating (1-5), calldrop_category (Satisfactory/Poor Voice Quality/Call Dropped), latitude, longitude, state_name

## Data Cleaning

Cleaning was done in Python (Pandas) — see [`scripts/clean_telecom_data.py`](scripts/clean_telecom_data.py). Key steps:

- Replaced the `-1` sentinel value in latitude/longitude with proper missing values (NaN)
- Relabelled `"NA"` in state_name to `"Not Available"` for clarity
- Trimmed stray whitespace across all text columns
- Kept `"Unknown"` as its own category in network_type (15.5% of rows) rather than dropping it, since it was too large a share of the data to discard
- **Kept exact duplicate rows (~60% of the dataset)** rather than removing them. Since this is crowdsourced call-quality data, repeated identical readings are plausible (multiple users logging similar conditions), rather than a clear collection error — so duplicates were kept as a deliberate judgment call rather than assumed to be a glitch.
- Since more than half the rows had no valid coordinates, the analysis was scoped to non-geographic patterns (operator, network type, usage context) rather than geographic/map-based analysis.

## Dashboard

Built in Power BI, with a custom dark, network-themed background and an operator slicer styled as a phone mockup.

![Dashboard screenshot](images/dashboard_screenshot.png)

**KPIs:** Total records, overall call drop rate, average rating, worst performing operator (by drop rate)

**Charts:**
- Call drop by operator (stacked bar)
- Rating by network type (clustered bar)
- Network quality by usage context — indoor vs outdoor vs travelling (stacked bar)

## Key Findings

- **VI performs best overall** — the most-used operator in the dataset (315 records), with the lowest call drop rate (7.3%) and highest average rating (3.71/5). VI's 4G users gave the highest share of 5-star ratings.
- **BSNL is the clear underperformer** — despite only 60 records, it has a 38.3% call drop rate and the lowest average rating (1.72/5) of any operator. Most BSNL users were on 3G, and 42 of 60 rated their experience 1/5.
- **MTNL**, though a small sample (3 records), also showed weak performance — a 33.3% drop rate and 2.67 average rating.

## Tools Used

- **Python (Pandas)** — data cleaning
- **Power BI** — dashboard and visualization

## Repository Structure

```
telecom-network-quality-analysis/
├── README.md
├── data/
│   ├── January_MyCall_2022.csv          # raw dataset
│   └── January_MyCall_2022_cleaned.csv  # cleaned dataset
├── scripts/
│   └── clean_telecom_data.py            # data cleaning script
├── dashboard/
│   └── telecom_dashboard.pbix           # Power BI dashboard file
└── images/
    ├── dashboard_screenshot.png
    └── telecom_dashboard_background.png
```
