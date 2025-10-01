***Latest version is v20250930***

# NFDRS_FEMS

Tool that automates the download and processing of FEMS data for user-specified stations. For each selected fire danger index, it generates:

- Daily listings  
- Percentile breakpoint tables  
- Percentile lookup tables  
- NFDRS charts

Calculations are performed at three geographic scales:

- **GACC** (Geographic Area Coordination Center)  
- **PSA** (Predictive Service Area)  
- **Station**

## Input Requirements

Users must provide a [StationList.csv](https://github.com/mpanunto/NFDRS_FEMS/blob/main/StationList.csv) file as input, which specifies the stations to include in the analysis.

A sample CSV is included with default stations from the **Northern California Geographic Area Coordination Center (ONCC)**. This file can be edited to include any stations the user wishes to download and process.
